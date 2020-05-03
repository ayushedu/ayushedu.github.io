---
layout: post
title:  "Kafka Consumer using Akka-Core"
date:   2020-04-26 04:04:04
author: Ayush Vatsyayan
categories: scala
tags:	    scala
cover:  "assets/akka-kakfa-consumer.jpg"
---

I faced this issue when working on a project last week. So I had to add a Kafka consumer in the project in order to write an integration test case. Now kafka consumer is pretty straightforward when using akka-stream, but the project had an earlier version of Akka (2.4.8) on which akka-stream wasn't supported.

Being pretty new to Akka, I googled to see if I can find some akka core solution of creating a kafka consumer, but nothing existed except one using akka-streams. The only other solution was using a java while loop, wherein the code continuously consume messages. This approach is a correct solution, however I cannot use it because whole project is written in scala this seemed quite an ugly solution to me.

For the  solution I've used akka and then send the akka messages to start and stop the kafka message consumption. We can also use Akka FSM, which I will cover some other time.

Consider this java version of kafka consumer - here we are consuming kafka messages in the while loop.
```java
while (true) {
  ConsumerRecords<String, String> records = consumer.poll(100)
  for (ConsumerRecord<String, String> record: records)
    // consume here
}
```

Now to make it an akka code we have to replace this while loop with the	akka messages. For this	we will	define two scala case objects `CONSUME` and `STOP`.
```scala
case object CONSUME
case object STOP
```

First let's define properties for kafka consumer
```scala
import java.util.properties

val props = new Properties()
props.put("bootstrap.servers", "127.0.0.1:9909")
props.put("group.id", "test")
props.put("enable.auto.commit", "true")
props.put("auto.commit.interval.ms", "1000")
props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")
props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")
```

Let's define kafka consumer and the collection to hold consumed messages.
```scala
import org.apache.kafka.clients.consumer.KafkaConsumer

var consumedMsgs = Map[String, String]()
var consumer: KafkaConsumer[String, String] = null

override def preStart() {
  consumer = new KafkaConsumer(props)
  consumer.subscribe(List("some topic").asJava)
}
```

Now we will define our consumer	to receive `CONSUME` and `STOP` messages.
```scala
override def wrappedReceive: Receive = {
  case CONSUME => 
    val records = consumer.poll(100)
    records.asScala.foreach { x =>
        consumedMsgs += (x.topic -> x.value)
    }

    self ! CONSUME // keep consuming messages

  case STOP => // do nothing for now
}
```


Now to mimic while loop	in scala we are sending `CONSUME` to self. This will be trigerred as soon as client code sends `CONSUME`.
We are receiving messages now, but we need to stop consuming messages when client wants. For this client will send the `STOP` message and we will make consumer as null.
To stop consuming messages we will also have to add a null check when consuming `CONSUME` message. Here is how it looks now

```scala
override def wrappedReceive: Receive = {
  case CONSUME if consumer != null => // consumer only if consumer isn't null
    val records = consumer.poll(10)
    records.asScala.foreach { x =>
        consumedMsgs += (x.topic -> x.value)
    }

    self ! CONSUME // keep consuming messages

  case STOP =>
    if (consumer != null) {
      consumer.close()
      consumer = null // set consumer to null
    }
}
```

This is a pretty straightforward way but works for a project where scala and akka are used.