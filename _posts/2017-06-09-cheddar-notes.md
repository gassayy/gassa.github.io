---
layout: post
title:  Real-Time Scheduling with Cheddar
author: gassa
category: Notes
tags: [cheddar, sheduling analysis, real-time]
---

[Cheddar](https://beru.univ-brest.fr/~singhoff/cheddar/) is an analysis tool
for real-time schedulability developed by [a research lab at the University of
Brest](http://www.lab-sticc.fr/en/index/). Recently, I use Cheddar for
application schedulability in my research. This post documents about how to use
the real-time scheduling simulation with temporal constrains for task
scheduling and buffer sharing in Cheddar.


## Background Knowledge

There are a number of literatures that have been studied the schedulability
analysis in the past decades, the section of the scheduling reference has
listed a number of papers in Cheddar's help section. This post only discusses a
few necessary concepts needed in my research.

First of all, ***each task T<sub>i</sub> is modelled by four temporal
parameters***:

* R<sub>i</sub> - the first release time of the task.
* C<sub>i</sub> - the worst case execution time(WCET) of the task, which is the
  highest computation time of the task.
* D<sub>i</sub> - the relative deadline of the task, which is the maximum
  acceptable delay between the release and the completion of any release of the
  task.
* P<sub>i</sub> - the period of the task.

The necessary condition for a system to be ***schedulable***: if a system has n
tasks (T<sub>1</sub>, ..., T<sub>n</sub>), the system utilization factor,
defined as $$U = \sum_{i=1}^{n} \frac{C_i}{P_i}$$, is less than or equal to 1.
The system utilization factor has the following properties:

Cheddar provides ***scheduling simulation*** and ***feasibility test***. The
scheduling simulation of Cheddar consists of a number of real-time schedulers,
such as earlist deadline first, deadline monotonic, least laxity and POSIX
scheduler with SCHED_FIFO, SCHED_RR and SCHED_OTHER. For any given task set,
three feasibility tests are applied:

(1) ___the processor utilization factor___, comparing the unilization factor of
a processor to a given bound, *i.e.*, $$ U \leq n(2^{1/n}-1)$$;

(2) ___the worst resonse time___ [1], the maximum time between the time the
task is ready to run and the time the task ends its execution for each release
sequence (*seq*), which is computed as $$ R_{i} = \displaystyle
\max_{seq=0,1,2...} (J_{i} + B_{i} + w_{i}(seq) - seq * P_{i}) $$, where
J<sub>i</sub> is a bound on the task wake up time jitter, B<sub>i</sub> is a
bound of blocking time on shared resources, $$ w_{i}(seq) = (seq+1)C_{i} +
\displaystyle \sum_{\forall j \ni hp(i)} \lceil \frac{J_{j} +
w_{i}(seq)}{P_{j}} \rceil * C_{j} $$ and $$ \forall seq: w_{i}(seq) \geq
(seq+1) * P_{i} $$.

Notice that hp<sub>i</sub> is all tasks with a priority greater than
task<sub>i</sub>. w<sub>i</sub>(seq) includes the time required for executing
task<sub>i</sub> with *seq* times and the worst case time for a
task<sub>i</sub> interfered by task<sub>j</sub> is computed as $$\frac{J_{j} +
w_{i}(seq)}{P_{j}} * C_{j} $$.

(3) ___the buffer utilization factor___ for a buffer shared by different tasks




## Cheddar Memo


---
### References:
[1] Applying new scheduling theory to static priority pre-emptive scheduling by
N. Audsley, A. Burns, M. Richardson, K. Tindell and A.J. Wellings
