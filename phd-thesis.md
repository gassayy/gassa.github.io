---
layout: page
title: PhD. Thesis
---

Android is a versatile and attractive environment for developing embedded
systems with a set of applications that is growing well beyond the original
market segment its designers had envisioned. The medical industry has invested
significant resources in exploring Android as a platform for on-demand,
personalized medical devices, *e.g.*, handicap assistive devices, heart
monitors, glucose analyzers, fall detectors. The robotic community is also
developing control or sensing applications on Android-equipped smartphones,
including autopilot control, audio-based indoor localization, harmonized sound
reproduction and *etc*.

___RTDroid___ presents a real-time extension on Android for real-time
applications, which require strict timing guantantee while still leveraging
existing functionalities on Android. More specifically, RTDroid provides a
real-time software stack seperating from the original Android stack and
isolates intererences from non-real-time (Non-RT) apps to real-time (RT) apps
while still enabling interaction. Thereby, the development of real-time apps
can utilize existing functionalities of Android on RTDroid. RTDroid leverages a
real-time kenel, a real-time Java virtual machine and redesgined real-time
framework.


### Real-Time Kernel and Real-Time Java Virtual Machine:

We ___partially___ patched the Android customised Linux kernel with Linux-RT
patch, including real-time scheduling plocies, synchronisation primitives. We
enable priority awareness in task scheduling and locking as well as basic
syscalls for user spaces.


We have ported an existing real-time Java virtual machine, FijiVM, on
Android-based Linux kernel for the real-time runtime environment. It provides
basic real-time Java programming APIs for the development of RTDroid framework
and has an Ahead-Of-Time (AOT) compiler for static compilation which takes
source codes of RTDroid and generates a native binary file.


### Real-Time Programming Model

RTDroid provides a redesigned real-time framework implementation for
application abstraction and communication APIs. It presents our solution as a
real-time programming model that extends Android's baisc application components
for real-time development. Our programming model consists of real-time
components for application expressiveness, real-tiime channels for interaction
and real-time menifest schema for component declarations.

To prevent interferences from the runtime memory management, we alter the
conventional garbage collection to an on-stack memory management inspired by
the scope memory of RTSJ. The on-stack memory management perseves memory bound
for object allocation for each component, which assures constant cost for
memory reclamation and prevents dangling reference by using runtime assginment
checks.

From a developer's point of view, our real-time components extend Android
components for application development, the real-time application manifest is
similar to the Android manifest for component declaration and real-time
parameter configuration. Thereby, the real-time parameters are used for
scheduling validation and model checking.


### Static Application Validation

RTDroid integrates its application manifest with Cheddar*, a
real-time scheduling analysis tool, for static application validation. We
utilize real-time parameters configured in the RTDroid’s application manifest
and feed these parameters into Cheddar for feasibility analysis, including
schedulability, memory usage and communication channel overflow.


More details can be found at [RTDroid website](http://rtdroid.cse.buffalo.edu)
and the full version of my thesis will be available soon.

---
\* For more information about
[Cheddar](http://beru.univ-brest.fr/~singhoff/cheddar/).

