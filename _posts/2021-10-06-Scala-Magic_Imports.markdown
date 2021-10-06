---
layout: post
title:  "Magic of Scala Imports"
date:   2021-09-10 04:04:04
author: Ayush Vatsyayan
categories: Scala
tags:	    scala
---
==== DRAFT ====
For a java developer scala is pretty much familiar - you can code without going deep into the details, which can do the job. But magic happens when you explore deep into it. Once such thing is the import statment.
From java perspective it looks pretty normal, but are certain changes which excite me.

We all know that import caluse can import packages and their members. Consider the below code
```
package com.example.helper

object JsonHelper {
  val uri = "http://somerandomuri"
  def sendSingleRequest = "Successfully send single request"
  def sendMultitpleRequest = "Successfully send multiple requests"
}
```
```
package com.example.security

object Authenticator {
  val userName = "guest"
  def authenticate = true // Successfully authenticated
}
```
```
import com.example.helper.JsonHelper
import com.example.helper.Authenticator

object RequestProcessor {
  def process = {
    // authenticate
    println(s"Authenticating on ${Authenticator.userName} and sending request on ${JsonHelper.uri}")
    if (Authenticator.authenticate) JsonHelper.sendMultitpleRequest
    else JsonHelper.sendSingleRequest
  }
}
```
Here in `RequestProcessor` we are using the functions from `JsonHelper` and `Authenticator` objects. Since they are in different packages, we have added import statements.
Now these java style import statments can be replaced by more convinient ones

```
object RequestProcessor {
    import programming.in.scala.traits.authenticator.Authenticator._ // access to all members in object
    import programming.in.scala.traits.helper._ // acess to all members in package
    
    def process = {
      // authenticate
      println(s"Authenticating on ${userName} and sending request on ${JsonHelper.uri}")
      if (authenticate) JsonHelper.sendMultitpleRequest
      else JsonHelper.sendSingleRequest
    }
  }
```




