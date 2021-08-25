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

```
sealed abstract class List[+A] extends AbstractSeq[A]
                                  with LinearSeq[A]
                                  with Product
                                  with GenericTraversableTemplate[A, List]
                                  with LinearSeqOptimized[A, List[A]]
                                  with Serializable
```
```
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

```
def immutableList(): Long = {
    val start = System.currentTimeMillis()
    val range = 1 to 500000

    var l1 = List[Int]()
    range.foreach {
    	i =>		  
	l1 = i :: l1 // pre-append the element to list
    }
    System.currentTimeMillis() - start
}

def mutableList(): Long = {
    val start = System.currentTimeMillis()
    val range = 1 to 1000000

    //var l1 = List.newBuilder[Int]
    val l1 = collection.mutable.ListBuffer.empty[Int]
    range.foreach {
    	i =>		  
	l1 += i
    }
    System.currentTimeMillis() - start
}

val time = if(args(0) == "mutable") mutableList else immutableList

println(s"Took $time milliseconds for ${args(0)}.")
```
On random run the mutable one took `1084 milliseconds`, while immutable with pre-append took `579 milliseonds`

So, for adding element to list, the immutable one is faster than mutable one, along with the functional programming methods exposed.
So design wise it makes more sense to use immutable list as var as mark it as private.
