---
layout: post
title:  "Inefficient Cassandra Query"
date:   2023-06-03 04:04:04
author: Ayush Vatsyayan
categories: Scala
tags:	    scala
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