---
layout: post
title:  "Scala List or ListBuffer"
date:   2021-08-25 04:04:04
author: Ayush Vatsyayan
categories: Scala
tags:	    scala
---

Every time I wanted to use scala collection, a question would popup - whether to use immutable collection as var or use mutable collection. 
As per scala collection performance they seem pretty identical, but still the question is what exactly is difference, however minor it may be.

First thing I did it deep dive into scala collection code of List and ListBuffer. List extends Abstract and linear seq while Listbuffer extends buffer, which means there are lot of function such as head, tail, foldLeft, flatMap are missing from mutable one.

```scala
sealed abstract class List[+A] extends AbstractSeq[A]
                                  with LinearSeq[A]
                                  with Product
                                  with GenericTraversableTemplate[A, List]
                                  with LinearSeqOptimized[A, List[A]]
                                  with Serializable
```
```scala
final class ListBuffer[A]
      extends AbstractBuffer[A]
         with Buffer[A]
         with GenericTraversableTemplate[A, ListBuffer]
         with BufferLike[A, ListBuffer[A]]
         with Builder[A, List[A]]
         with SeqForwarder[A]
         with Serializable
```


To test which one is faster, I wrote a small scala script.

````scala
def addToListBuffer(range: Range): Int = {
  val buffer = collection.mutable.ListBuffer.empty[Int]
  range.foreach(buffer += _)
  buffer.size
}

def addToList(range: Range): Int = {
  var list = List[Int]()
  range.foreach(e => list = e :: list)
  list.size
}

val start = System.currentTimeMillis()
val range = 1 to args(1).toInt

val size = if(args(0) == "mutable") addToListBuffer(range) else addToList(range)
val time = System.currentTimeMillis() - start

println(s"Took $time milliseconds for ${args(0)} for $size elems.")
```

The performance, as expected, is identical, but due to functional programming methods exposed it makes more sense to use immutable List as private var.
