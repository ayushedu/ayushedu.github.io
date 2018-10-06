---
layout: post
title:  "Setting up Spark Development Environment"
date:   2018-06-04 08:43:59
author: Ayush Vatsyayan
categories: Apache-Spark
tags:	    spark
cover:  "/assets/instacode.png"
---

Now that you know [What is Spark](https://ayushedu.github.io/apache-spark/2018/04/18/Apache-Spark-Introduction.html), we'll see how to set up and test a Spark development environment on Windows, Linux (Ubuntu), and macOS X — whatever common operating system you are using, this article should give you what you need to be able to start developing Spark applications.

# What is Spark development environment?
The development environment is an installation of [Apache Spark](https://spark.apache.org) and other related components on your local computer that you can use for developing and testing Spark applications prior to deploying them to a production environment.

Spark provides support for Python, Java, Scala, R. Spark itself is written in Scala, and runs on the [Java Virtual Machine (JVM)](http://www.oracle.com/technetwork/java/javase/downloads/index.html), so to run Spark all you need is an installation of Java. 
However, if you want to use the Python API (PySpark), you will also need a [Python interpreter](https://www.python.org/downloads/) (version 2.7 or later). Or if you want to use R (SparkR), you have to install a version of [R](https://cloud.r-project.org) on your machine.

# What are the spark setup options
The options for getting started with Spark are:
* Downloading and installing [Apache Spark](https://spark.apache.org) components individually on your laptop.
* Downloading the quick start VM distribution.
* Running a web-based version in [Databricks Community Edition](https://databricks.com/try-databricks), a free cloud environment.
I'll explain all these options to you.

## Installing Manually
### Downloading Spark Locally
To download and run Spark locally, the first step is to make sure that you have Java installed on your machine, along with Python/R version if you would like to use Python or R. Next, visit the project’s official [download page](https://spark.apache.org/downloads.html) and select the package type “Pre-built for Hadoop 2.7 and later,” and click “Direct Download.” This will download a compressed TAR file, or tarball.

### Building Spark from Source
You can also build and configure Spark from source. You can select a [Spark source package](https://github.com/apache/spark) from Github to get just the source and follow the instructions in the [README](https://github.com/apache/spark/blob/master/README.md) file for building.

#### Installation/Configuration Steps
In case you choose to install Spark manually I suggest to use Vagrant, which will provide the isolated environment in your host OS and prevent the the host OS from getting corrupted.
The detailed steps are available on [Github](https://github.com/ayushedu/sparkvagrant).

## Downloading the quick start VM or Distribution
You can download the quick start VM from [Hortonworks](https://hortonworks.com/products/sandbox/) or [Clouders](https://www.cloudera.com/downloads/quickstart_vms/5-13.html). These distributions are virtual images, hence to use them you need to install VMware or Oracle Virtualbox. These VM images are pre-configured, hence you don't have to perform any additional installation or configuration.
For distributions you can choose [Hortonworks](https://hortonworks.com/products/data-platforms/hdp/), [Cloudera](https://www.cloudera.com/downloads/spark2/2-3.html), or [MapR](https://mapr.com/try-mapr/).

## Running Spark in the Cloud
Databricks offers a free community edition of its cloud service as a learning environment. You need to [Sign Up for the community account](https://accounts.cloud.databricks.com/registration.html#signup/community] and follow the steps.

# Testing the intallation
Once we have installed the Apache Spark, we need to test the installation.
* Java version: `java -version`
* sbt version: `sbt about`
* Hadoop: `hdfs -version` and `hdfs dfs -ls .`
* Python version: `python --version`
* Execute Pyspark: `pyspark` on console and you will be logged into the Spark shell for python
```
>>> sc
>>> sc.version
```
* Execute Spark-Shell: Type `spark-shell` on console and you will be logged into Spark shell for scala
```
scala> sc
scala> sc.version
```

## Terminal Recording
<a href="https://asciinema.org/a/204919" target="_blank"><img src="https://asciinema.org/a/204919.png" /></a>

# Summary
You now have a Spark development environment up and running on your computer.

So far we have tested our spark environment. In the next article we expand on this process, building a simple but complete data cleaning application.