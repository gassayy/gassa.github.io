---
layout: post
title: Scala - Future Callbacks
author: gassa
comments: true
category: Scala
tags: [scala, future]
published: false
---


This article continues about Scala Future in [my previous article](2013-07-30-scala-future.html) and dives deep into exceptions, monadic operations and composition in Scala Future.



### Handling Exceptions


#### Using the Try Type


#### Functional Composition on Futures 

```
object LearningFuture extends App {

  def fn(id: Int, sleepSec: Int): Future[String] = Future {
    TimeUnit.SECONDS.sleep(sleepSec)
    println(s"future #$id")
    s"future #$id"
  }

  override def main(args: Array[String]): Unit = {
    val f1 = fn(1, 3)
    val f2 = fn(2, 1)
    val f3 = fn(3, 2)

    val f4 = Future.sequence(List(f1, f2, f3))

//    val f4 = for {
//      r1 <- f1
//      r2 <- f2
//      r3 <- f3
//    } yield (r1, r2, r3)

    val rs = Await.result(f4, 4.seconds)
  }
}
```
