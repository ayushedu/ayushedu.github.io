---
layout: post
title:  "5 Improvements and Features in Spark 2.4.0"
date:   2019-01-20 13:24:00
author: Ayush Vatsyayan
categories: Spark
tags:	    spark
cover:  "/assets/apache_spark_logo_big_0.png"
---
# Spark Release 2.4.0
[Apache Spark][1] 2.4.0 is the spark's latest version - the fifth release in 2.x line -  released in Nov, 2018. This release has some major features such as: Barrier Execution Mode for better integration with deep learning frameworks, 30+ built-in and higher-order functions to deal with complex data type easier, improved the K8s integration, along with experimental Scala 2.12 support. 
Other major updates include the built-in Avro data source, Image data source, etc.
 
In this post we will be walking through 5 major features/improvements in Core and MLlib module of Spark 2.4.0:
1. [Support for Barrier Execution Mode in the scheduler](#bem).
2. [Support for Scala 2.12](#s2s)
3. [Built-in Higher-order functions](#hof)
4. [Built-in Avro data source](#bads)
5. [Spark datasource for image format](#sdif)

## Improvements and New Features

### #1. Barrier Execution Mode {#bem}
BigData and AI gave rise to several frameworks like [Apache Spark][1], [TensorFlow][2] and [Apache MXNet][3]. 
Each of these frameworks doesn't fully address the end-to-end distributed training workflow. On one hand [Apache Spark][1] is a successful unified analytics engine, while on the other hand frameworks like [TensorFlow][2] and [Apache MXNet][3] are good in AI.

For e.g. Consider a scenario: 

> *Building a pipeline that fetches training events from production datawarehouse such as [Apache Hive](https://hive.apache.org) or [Redshift](https://aws.amazon.com/redshift/) from AWS and fit a Deep Learning (DL) model with data parallelism.*

So to connect the dots for scenario, such as above, [Apache Spark][1] has added a new execution model called as **Barrier Execution Mode**, which launches the tasks at the same time and provides users enough information and tooling to embed distributed DL training into a Spark pipeline.

For example, [Horovod](https://github.com/uber/horovod), which is a distributed training framework for [TensorFlow][2] and [MXNet][3], uses [Message Passing Interface](https://en.wikipedia.org/wiki/Message_Passing_Interface) (MPI) to implement all-reduce to accelerate distributed TensorFlow training, which is different from [MapReduce](https://en.wikipedia.org/wiki/MapReduce) used by Spark. 

In Spark, a task in a stage doesn’t depend on any other tasks in the same stage, and hence it can be scheduled independently, but in MPI all workers start at the same time and pass messages around so as to provide users enough information and tooling to embed distributed DL training.

### #2. Scala 2.12 Support {#s2s}
[Scala 2.12][4] was release in November 2016, since then spark wasn't supporting it. This was mainly due to two major issues blocking the release:
#### Issue 1: Spark’s current Dataset API is broken for users of Java 8 lambdas or Scala 2.12
The current API had overloaded methods whose signatures were ambiguous when resolving calls that use the Java 8 lambda syntax.
The Spark API had overloaded methods of the following form:
```scala
class Dataset[T] {
  def map[U : Encoder](func: T => U): Dataset[U] = ...
  def map[U](func: MapFunction[T, U], encoder: Encoder[U]): Dataset[U] = ...
}
```
For Scala clients, this works fine for 2.12, however there is an **issue for Java clients using this API**. In the Java bytecode, the API has these overloaded methods:
```Java
<U> Dataset<U> map(scala.Function1<T,U> f, Encoder<U> e) { ... }
<U> Dataset<U> map(MapFunction<T, U> f, Encoder<U> e) { ... }
```
If the API is compiled with Scala 2.11, Java 8 code can use a lambda: d.map(x -> y, enc). The Java compiler will select the second overload, because the first one takes a Function1, which is not a type of [functional interface](https://docs.oracle.com/javase/specs/jls/se8/html/jls-9.html#jls-9.8), so it is not applicable to a lambda.

If the API is compiled with Scala 2.12, scala.Function1 is now type of [functional interface](https://docs.oracle.com/javase/specs/jls/se8/html/jls-9.html#jls-9.8). This makes the invocation in Java code ambiguous.

#### Issue 2: Support for the new lambda encoding to the closure cleaner
When Scala constructs a [closure](https://stackoverflow.com/a/7464475/6065591), it determines which outer variables the closure will use and stores references to them in the closure object. This allows the closure to work properly even when it's called from a different scope than it was created in.

Scala sometimes errs on the side of capturing too many outer variables. That's harmless in most cases, because the extra captured variables simply don't get used (though this prevents them from getting garbage collected). But it posed a problem for Spark, which had to send closures across the network so they can be run on slaves. When a closure contains unnecessary references, it wastes network bandwidth. More importantly, some of the references may point to non-serializable objects, and Spark will fail to serialize the closure.

To work around this bug in Scala, the [ClosureCleaner](https://github.com/apache/spark/blob/master/core/src/main/scala/org/apache/spark/util/ClosureCleaner.scala) traverses the object at runtime and prunes the unnecessary references. Since it does this at runtime, it can be more accurate than the Scala compiler can. Spark can then safely serialize the cleaned closure. For e.g.
```scala
def filter(f: T => Boolean): RDD[T] = withScope {
  val cleanF = sc.clean(f)
  new MapPartitionsRDD[T, T](
    this,
    (context, pid, iter) => iter.filter(cleanF),
    preservesPartitioning = true)
}
```
[Scala 2.12][4] **generates closures differently, similar to Java 8 lambdas**. The current closure cleaner is not designed for such closures. This was one of the remaining issues that blocked the support of Scala 2.12 in Spark.

### #3. Higher-order Functions {#hof}
Added a lot of new built-in functions, **including higher-order functions, to deal with complex data types easier**. This is done to improve compatibility with the other data processing systems, including [Hive](https://hive.apache.org), [Teradata](https://www.teradata.com/Products/Software/Database), [Presto](https://prestodb.github.io), [Postgres](https://www.postgresql.org), [MySQL](https://www.mysql.com), [DB2](https://www.ibm.com/analytics/us/en/db2/), [Oracle](https://www.ibm.com/analytics/us/en/db2/), and [MS SQL Server](https://www.microsoft.com/en-ie/sql-server/sql-server-downloads).

### #4. Built-in Avro Data Source {#bads}
[Apache Avro](https://avro.apache.org) is a popular data serialization format. It is widely used in the Spark and Hadoop ecosystem, especially for Kafka-based data pipelines. In previous releases Spark SQL can read and write the avro data using the external package https://github.com/databricks/spark-avro. In 2.4 the **support for spark-Avro is now built-in to provide better experience for first-time users of Spark SQL and structured streaming**, and to further improve the adoption of structured streaming.

### #5. Spark Datasource for Image Format {#sdif}
Deep Learning applications commonly deal with image processing. A number of projects, such as [MMLSpark](https://github.com/Azure/mmlspark), [TensorFlowOnSpark](https://github.com/yahoo/TensorFlowOnSpark), [DeepLearning4J](https://deeplearning4j.org), etc., add some Deep Learning capabilities to Spark, but they struggle to communicate with each other or with MLlib pipelines because there is no standard way to represent an image in Spark DataFrames.

The change is done to **represent images in Spark DataFrames and Datasets (based on existing industrial standards), and an interface for loading sources of images**. It is not meant to be a full-fledged image processing library, but rather the core description that other libraries and users can rely on.

The image format is an in-memory, decompressed representation that targets low-level applications. It is significantly more liberal in memory usage than compressed image representations such as JPEG, PNG, etc., but it allows easy communication with popular image processing libraries and has no decoding overhead.
```shell
scala> val df = spark.read.format("image").option("dropInvalid", true).load("data/mllib/images/origin/kittens")
df: org.apache.spark.sql.DataFrame = [image: struct<origin: string, height: int ... 4 more fields>]

scala> df.select("image.origin", "image.width", "image.height").show(truncate=false)
+-----------------------------------------------------------------------+-----+------+
|origin                                                                 |width|height|
+-----------------------------------------------------------------------+-----+------+
|file:///spark/data/mllib/images/origin/kittens/54893.jpg               |300  |311   |
|file:///spark/data/mllib/images/origin/kittens/DP802813.jpg            |199  |313   |
|file:///spark/data/mllib/images/origin/kittens/29.5.a_b_EGDP022204.jpg |300  |200   |
|file:///spark/data/mllib/images/origin/kittens/DP153539.jpg            |300  |296   |
+-----------------------------------------------------------------------+-----+------+
```

[1]: https://spark.apache.org
[2]: https://www.tensorflow.org
[3]: https://mxnet.apache.org
[4]: https://www.scala-lang.org/news/2.12.0/