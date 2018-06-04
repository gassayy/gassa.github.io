---
layout: post
title: Scala - Future
author: gassa
comments: true
category: Scala
tags: [scala, future]
---


_Updated @ May, 8th 2018_

This article was writen in 2013, it briefly documents what I have learned about Scala Future.
 
By the definition, The `Future[T]` type (future type ) is a ***container*** type that presents a block of asynchronous computations (future computations) resulting in a value of type T (future value).

```scala
traits Future[T]
def apply[T](b: =>T)(implicit ex: ExecutionContext): Future[T]
```

- The `Future[T]` is defined as a scala trait, similar to Java interface. 
- The future computation is started by calling the `apply` method on the `Future` companion object.


The `apply` method takes a ***by-name*** parameter of the type [T]. Developers can start a future computation with an anonymous function, e.g., `val f = Future { "Hello World" }`. While the `apply` mehtod also takes an implicit ExecutionContext parameter, analogous to the Java's `Executor`, It abstracts over where and when threads executed.

### Execution Context

The `scala.concurrent` package defines the `ExecutionContext` as a scala trait with a companion object. The companion object implements an execute(Runnable) method and a reportFailure(Throwable) method:

+ ***executor(Runnable)*** is similar to the execute() function of the Java Executor, which takes a runnable object as a parameter and schedules the runnable with its context.
+ ***reportFailure(Throwable)*** takes a `Throwable` object, it is called whenever some task throws an exception. 

In Scala, the `ExecutionContext` companion object contains an default execution context named ***global***, which internally uses a ForkJoinPool instance. In practice, the creation of `ExecutionContext` is normally  transparent to developers since application framework or the management component of the execution environment may abstract it away. For example, developers can import the default global execution context in the `scala.concurrent` package.

For learning purpose, The following code <sup>[1](#footnote1)</sup> shows how to fork an execution context with two thread worker and create a new execution context by calling the `fromExecutorService` method.

```scala
object ExecutionContextCreate extends App { 
  val pool = new forkjoin.ForkJoinPool(2)
  val ctx = ExecutionContext.fromExecutorService(pool)
  etc.execute(new Runnable { 
  	def run() = println("Running on the execution context")
  })
  Thread.sleep(5000)
}
/* A more concise version */
def execute(body: =>Unit) = ExecutionContext.global.execute( 
  new Runnable { def run() = body } 
) 
```


### Future Callbacks

There are a number of callbacks defined in the `Future` companion object, all callbacks are non-blocking functions. After a callback is installed to a future computation, it gurantee to be invoked eventually, as the following code shows.

```scala
import scala.concurrent.ExecutionContext.Implicits.global 

object FutureExample1 extends App {
  override def main(args: Array[String]): Unit = {
    val f = Future {
      TimeUnit.SECONDS.sleep(5)
      println("Hello Future")
    }
    println("Hello World")
    
    //Non blocking callback
    f.onComplete { case _ => println }
    println("Future.onComplete is non-blocking!")
    Thread.sleep(10000)
  }
}
```

<a name="footnote1">[1]</a> The code example is from a book called "Learning Concurrent Programming in Scala" (http://www.packtpub.com/application-development/learning-concurrent-programming-scala-second-edition)

