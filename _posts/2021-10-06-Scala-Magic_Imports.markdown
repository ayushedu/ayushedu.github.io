---
layout: post
title:  "Flexible Scala Import"
date:   2021-09-10 04:04:04
author: Ayush Vatsyayan
categories: Scala
tags:	    scala
---
For a java developer scala is pretty much familiar - you can code without going deep into the details. Which can do the job, but magic happens when you explore it and try to write code the scala way. Once such thing is scala's flexible import. 

We all know that import caluse can import packages and their members. Consider the below code
```
package com.example.helper

object JsonHelper {
  val uri = "http://somerandomuri"
  val version = "1.2"
  def sendSingleRequest = "Successfully send single request"
  def sendMultitpleRequest = "Successfully send multiple requests"
}
```
```
package com.example.security

object Authenticator {
  val userName = "guest"
  val version = "1.0"
  def authenticate = true // Successfully authenticated
}
```
```
import com.example.helper.JsonHelper
import com.example.security.Authenticator

object RequestProcessor {
  def process: Unit = {
    println(s"Authenticating ${Authenticator.userName} and sending request on ${JsonHelper.uri}")
    if (Authenticator.authenticate) JsonHelper.sendMultitpleRequest
    else JsonHelper.sendSingleRequest
  }
}
```
Here in `RequestProcessor` we are using the functions from `JsonHelper` and `Authenticator` objects. Since they are in different packages, we have added import statements.

Now these java style import statments can be replaced by more flexible and convinient ones

```
object RequestProcessor {
    import com.example.security.Authenticator._ // access to all members in object
    import com.example.helper._ // acess to all members in package
    
    def process: Unit = {
      println(s"Authenticating on ${userName} and sending request on ${JsonHelper.uri}")
      if (authenticate) JsonHelper.sendMultitpleRequest
      else JsonHelper.sendSingleRequest
    }
  }
```

So far it's quite similar to java's `*`. But there are two advanced features provided on top of this:
1. Unlike java, import statements can appear anywhere inside the code. 
2. Imported members can be renamed
3. Imported member can be hidden

## Import can appear anywhere in code
Above code for `RequestProcessor` can be converted to 
```
object RequestProcessor {
    def process = {
      import com.example.security.Authenticator._
      import com.example.helper._
      
      println(s"Authenticating on ${userName} and sending request on ${JsonHelper.uri}")
      if (authenticate) JsonHelper.sendMultitpleRequest
      else JsonHelper.sendSingleRequest
    }
  }
  ```

## Renaming imported members
For e.g while importing `Authenticator` can be renamed to `AuthService`
```
object RequestProcessor {
    def process: Unit = {
      import com.example.security.{Authenticator => AuthService}
      import com.example.helper.JsonHelper

      println(s"Authenticating on ${AuthService.userName} and sending request on ${JsonHelper.uri}")
      if (AuthService.authenticate) JsonHelper.sendMultitpleRequest
      else JsonHelper.sendSingleRequest
    }
  }
  ```
 
## Hiding a member
Hidding a member in a class can be required if there are conflicting member names in the imported classes. For e.g. if we want to print `JsonHelper.version`, then while importing, we will have to hide `Authenticator.version` 

```
object RequestProcessor {
    def process: Unit = {
      import com.example.security.Authenticator.{version => _, _}
      import com.example.helper.JsonHelper._

      println(s"Authenticating on $userName and sending request on $uri against version: $version")
      if (authenticate) sendMultitpleRequest
      else sendSingleRequest
    }
  }
```

This hiding feature can come handy when we have third party imports having conflicting member names.
