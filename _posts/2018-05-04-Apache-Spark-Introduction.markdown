---
layout: post
title:  "Apache Spark Introduction"
date:   2018-05-04
author: Ayush Vatsyayan
categories: "Apache Spark" "BigData"
tags:	spark bigdata
cover:  "/assets/instacode.png"
---
In this first Spark article, I'll try to answer the question "What is Spark?" and give you an in-depth overview of what makes it special. I'll outline the main features, including some of the advanced functionality. I'll also show you some of the main building blocks.

# What is Spark?

[Apache Spark](https://spark.apache.org) is an open-source **cluster-computing** framework that provides unified analytics engine for large-scale data processing. Originally developed at the University of California, Berkeley's AMPLab in 2009, the Spark codebase was later donated to the [Apache Software Foundation](http://apache.org), which has maintained it since.

It provides high-level APIs in *Java*, *Scala*, *Python*, and *R* and an optimized engine that supports general execution graphs. It also supports a rich set of higher-level tools including [Spark SQL](https://spark.apache.org/docs/latest/sql-programming-guide.html) for **structured and semi-structured data processing**, [MLlib](https://spark.apache.org/docs/latest/ml-guide.html) for **machine learning**, [GraphX](https://spark.apache.org/docs/latest/graphx-programming-guide.html) for **graph processing**, and [Spark Streaming](https://spark.apache.org/docs/latest/streaming-programming-guide.html).

## Structured and Unstructured Data

Structured data is comprised of clearly defined data types whose pattern makes them easily searchable; while unstructured data – “everything else” – is comprised of data that is usually not as easily searchable, including formats like audio, video, and social media postings.

Semi-structured data maintains internal tags and markings that identify separate data elements, which enables information grouping and hierarchies. Example include JSON and XML.

## Machine Learning

[Machine learning](https://en.wikipedia.org/wiki/Machine_learning) is a method of data analysis that automates analytical model building. It is a branch of artificial intelligence based on the idea that systems can learn from data, identify patterns and make decisions with minimal human intervention. One of the example of Machine Learning that we daily use is [Google News](https://news.google.com/) that groups news stories into cohesive groups.

## Cluster Computing

A [computer cluster](https://en.wikipedia.org/wiki/Computer_cluster) is a set of loosely or tightly connected computers that work together so that, in many respects, they can be viewed as a single system. Cluster computing is used for high performance computing and high availability computing.

## Graph Processing

A [graph](https://en.wikipedia.org/wiki/Graph_theory) in data terms is simply a representation of individual entities and their relationships. They are now widely used for data modeling in application domains for which identifying relationship patterns, rules, and anomalies is useful.
The domains of Graph processing include the web graph, social networks, the Semantic Web, knowledge bases, protein-protein interaction networks, and bibliographical networks, among many others.

# Where did Spark come from?

Spark is one of Hadoop’s sub project developed by Matei Zaharia in UC Berkeley’s AMPLab in 2009, and Open Sourced in 2010 under a BSD license. In 2013 it was donated to [Apache software foundation](http://apache.org), and in Feb-2014 became a top level Apache project.


# How popular is Spark?

Spark is used at a wide range of organisations to process large datasets. As a powerful processing engine built for speed and ease of use, Spark lets companies build powerful analytics applications.

# Why companies use Apache Spark:

* 91% use Apache Spark because of its performance gains.
* 77% use Apache Spark as it is easy to use.
* 71% use Apache Spark due to the ease of deployment.
* 64% use Apache Spark to leverage advanced analytics.
* 52% use Apache Spark for real-time streaming.


## Features of Spark
Apache Spark has following features:

- *Speed*: Apache Spark achieves high performance for both batch and streaming data, using a state-of-the-art DAG scheduler, a query optimizer, and a physical execution engine. 
- *Ease of use*: Spark offers over 80 high-level operators that make it easy to build parallel apps. And you can use it interactively from the Scala, Python, R, and SQL shells. 
- *Generality*: Spark powers a stack of libraries including SQL and DataFrames, MLlib for machine learning, GraphX, and Spark Streaming. You can combine these libraries seamlessly in the same application.
- *Runs Everywhere*: You can run Spark using its standalone cluster mode, on EC2, on Hadoop YARN, on Mesos, or on Kubernetes. Access data in HDFS, Apache Cassandra, Apache HBase, Apache Hive, and hundreds of other data sources.

# Why Spark over Hadoop?
- *Speed*: Spark runs applications up to 100x faster in memory and 10x faster on disk than Hadoop by reducing the number of read/write cycle to disk and storing intermediate data in in-memory/cache whereas Hadoop reads and writes from disk.
- *Difficulty*: Sprk is easy to program as it has tons of high-level operators with RDD (Resilient Distributed Dataset) whereas in Hadoop you need to hand code each and every operation.

- *Easy to Manage*: Spark is a unified engine capable of performing batch, interactive and Machine Learning and Streaming all in the same cluster i.e. there is no need to manage different component for each need. On the other hand Hadoop only provides the batch engine, and we have to use different components/frameworks for all other tasks.

- *Real-time analysis*: Spark can process real time data, such as twitter feed,  using Spark Streaming. Whereas Hadoop was designed to perform only batch processing on voluminous amounts of data.

# What all can you do?
- *Spark SQL, Datasets, and DataFrames*: Processing structured data with relational queries.
- *Structured Streaming*: Processing structured data streams with relation queries.
- *Spark Streaming*: Processing data streams.
- *MLlib*: Applying machine learning algorithms.
- *GraphX*: Processing graphs.

# Summary
You should now understand Spark's main benefits, a little about its history, and roughly about each component. 

We haven't yet started on Spark environment and other concepts. In our next part we will cover the different Spark's interfaces and it's basic concepts such as RDD.