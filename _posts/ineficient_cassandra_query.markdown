---
layout: post
title:  "Inefficient Cassandra Query"
date:   2023-06-04 12:34:00
author: Ayush Vatsyayan
categories: Cassandra
tags:	    cassandra
---

The problem with taking over an existing project in between is there are always blindspots you aren't aware of.
This week a cassandra issue was reported on customer site which stated that a particular cassandra query was unable to execute successfully and was retrying indefinitely. 

When we started investigating we found that an inefficient select query was used to fetch expired data. The query was using `ALLOW FILTERING`, which wasn't recommended for production server. Also there were lot of tombstones, which also was the area of concern.

For reference consider the following schema

{% highlight sql %}
CREATE TABLE order_details (
    customer_id uuid,
    week_of_year smallint,
    order_id uuid,
    order_details blob,
    PRIMARY KEY ((customer_id, week), order_id)
)
{% endhighlight %}

The job responsible for deleting expired records worked in two steps. In first step the expired records are fetched using following select query

{% highlight sql %}
SELECT order_id, week_of_year FROM order_details WHERE week_of_year < ? LIMIT 10000 ALLOW FILTERING;
{% endhighlight %}

In next step the records are deleted one by one using following delete query

{% highlight sql %}
DELETE FROM order_details WHERE customer_id=? AND week_of_year=? AND order_id=?
{% endhighlight %}

# Issue with above approach
There are two issues with the above select query:
	1. The query to fetch expired data is inefficient, as it uses `ALLOW FILTERING` - something not recommended for production use. 
	2. The data is deleted row by row, which generates lot of tombstones. The tombstones build up more quickly than compaction process can handle.

## Why ALLOW FILTERING isn't recommended 
`ALLOW FILTERING` makes cassandra do a full scan of all partitions. Thus, returning the results experience a significant latency proportional to amount of data in table.  Cassandra works this up by retrieving all the data from table order_details and then filtering out the ones which are less than the week_of_year column.

If the dataset is small, the performance will be reasonable. Hence, cassandra throws a warning.  One way to make this efficient is getting all rows and then fitler them in your code. This is the same approach we took for the workaround script.

# Solution
Solution here is multifold along both query performance and tombstone generation

## Workaround/Hack for quick resolution
As a first step we had to provide a workaround or hack for customer server. For this we decided to provide a script to delete expired data. So we wrote a python script to fetch all data. Once we had all data we filtering it and then executed delete statement for each expired partition. 

{% highlight python %}
from cassandra.cluster import Cluster
from datetime import datetime, timedelta
import subprocess

DATA_RETENTION_DAYS = 365

# Get hostname
hostname = subprocess.check_output(['hostname','-i']).replace('\n','')

cluster = Cluster([hostname])
session = cluster.connect('order_db')

# Get earliest week code as YYWW
year, week, _ = (datetime.today() - timedelta(days=DATA_RETENTION_DAYS)).isocalendar()
earliest_week = int("{0}{1:02}".format(year % 100, week))

# Delete statement
delete_stmt = session.prepare("DELETE FROM oder_details WHERE customer_id=? AND week_of_year=?")

# Read data
rows = session.execute('SELECT DISTINCT customer_id, week_of_year FROM order_details')
expired_count = 0
count = 0

for row in rows:
  count = count + 1
  
  if(count % 10000 == 0):
    print 'Processed', count, 'with expired count', expired_count
  
  if(row.week < earliest_week):
    session.execute(delete_stmt,[row.customer_id, row.week_of_year])
    expired_count = expired_count + 1
    
cluster.shutdown()
print 'expired:', expired_count, ', valid:', count - expired_count
{% endhighlight %}

By executing delete against partition, instead of individual record, we reduced the tombstone generation by 88%. Something which will also go into our long term fix. However, in long term we will update schema to delete data by `week_by_year` only.

Also we increased the data retention period from an year to two years. This enabled the cassandra to return data.

## Fine tune compaction parameters
For the quick workaround we used the following compaction parameters to trigger compaction, as earlier the compaction wasn't taking place. 
{% highlight sql %}
ALTER TABLE order_details WITH compaction = 
{ 'class' :  'LeveledCompactionStrategy','unchecked_tombstone_compaction':true, 'tombstone_compaction_interval':60,'tombstone_threshold':0.01  } 
AND gc_grace_seconds = 60;

{% endhighlight %}

Currently the table is using all default value for compaction parameters. We will have to test and decide on the solution. The focus will be on finetuning following parameters
	• `unchecked_tombstone_compaction`
	• `tombstone_compaction_interval`
	• `tombstone_threshold`
	• `gc_grace_seconds`: We are using low gc_grace_seconds, as ghost data won't be a issue for the historical data.

## Decreasing tombstone generation
Currently the data is deleted row by row i.e. data is deleted by both partitioning key and clustering key. We will now be deleting data by partitioning key only, which will reduce the tombstone generation by 88% on the client data. For each partition deletion only single tombstone will be generated.

## Remove ALLOW FILTERING from query
To remove allow filtering we decided to change the table scheme to
{% highlight sql %}
CREATE TABLE order_details (
    customer_id uuid,
    week_of_year smallint,
    order_id uuid,
    order_details blob,
    PRIMARY KEY ((week_of_year), customer_id, order_id)
)
{% endhighlight %}

Here we removed `customer_id` from partition key. This will allow us to delete data by `week_of_year` only so that now we won't have to retrieve expired records from db to figure out customer_id and order_id. Now the delete query will be
{% highlight sql %}
DELETE FROM order_details WHERE week_of_year=?
{% endhighlight %}

### Handling solution upgrades for schema change
Now we need to migrate the users from old schema to new for cases where user upgrades instead of fresh install. The change in primary key of cassandra table is not supported via upgrade i.e. for any change in primary key the data has to be migrated from old table to new table. 

This part is yet to be worked on, but on initial thought we will work this using script to migrate data as it's a one-time process.

 

