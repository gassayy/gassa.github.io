---
layout: post
title:  Learning Note for Cheddar
author: gassa
category: Notes
tags: [cheddar, sheduling analysis]
---

[Cheddar](https://beru.univ-brest.fr/~singhoff/cheddar/) is an analysis tool
for real-time schedulability developed by [a research lab at the University of
Brest](http://www.lab-sticc.fr/en/index/). Recently, I use Cheddar for
application schedulability in my research. This post documents about how to use
the real-time scheduling simulation with task scheduling and communication
buffer in Cheddar.


## Background Knowledge

There are a number of literatures that have been studied the schedulability
analysis in the past decades, the section of the scheduling reference in
Cheddar has listed a number of papers. This post discusses only a few necessary
concepts.

First of all, ***each task T<sub>i</sub> is modelled by four temporal
parameters***:

* R<sub>i</sub> - the first release time of the task.
* C<sub>i</sub> - the worst case execution time(WCET) of the task, which is the
  highest computation time of the task.
* D<sub>i</sub> - the relative deadline of the task, which is the maximum
  acceptable delay between the release and the completion of any release of the
  task.
* P<sub>i</sub> - the period of the task.

A necessary condition for a system to be ***schedulable***: if a system has n
tasks (T<sub>1</sub>, ..., T<sub>n</sub>), the system utilization factor,
defined as $$U = \sum_{i=1}^{n} \frac{C_i}{P_i}$$, is less than or equal to 1.

***Scheduling policies***:

Rate-monotonic scheduling: The shorter the period, the higher the priority.


Static-priority scheduling and dynamic-priority scheduling


Rate Monotonic Scheduling v.s Earliest Deadline Firs



***Schedulability test***:


***Response Time analysis***:




## Cheddar Memo



