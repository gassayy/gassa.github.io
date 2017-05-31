---
layout: page
title: PhD. Thesis
---

RTDroid presents a real-time extension on Android for real-time applications,
which require strict timing guantantee while still leveraging existing
functionalities on Android. Our solution provides isolation and interaction
between real-time applications in RTDroid and non-real-time features in
Android. Thereby, RTDroid isolates non-real-time interferences by using a
completely seperated software stack for real-time applications.


* __The application framework layer__: RTDroid provides a redesigned real-time
  framework implementation for application abstraction and communication
  interfaces. We present our solution as a real-time programming model and
  extend Android's application abstraction for real-time features. The
  programming model consists of real-time components for application
  expressiveness, communication APIs for interaction and menifest schema for
  component declaration.

  From a developer's point of view, our real-time components extend Android
  components for application development, the real-time application manifest is
  similar to the Android manifest for component declaration and real-time
  parameter configuration. Thereby, the real-time parameters are used for
  scheduling validation and model checking.

* __The runtime layer__: We have ported an existing real-time Java virtual
  machine, FijiVM, on Android-based Linux kernel for the real-time runtime
  environment. It has an Ahead-Of-Time compiler that takes Java bytecodes of a
  real-time application and application manifest file, runs scheduling analysis
  and generates a binary executable for the targeting device.

  To prevent interferences from memory management, we alter the conventional
  garbage collection to an on-stack memory management inspired by the scope
  memory of RTSJ. The on-stack memory management perseves memory bound for
  object allocation in each component, assures constant cost for memory
  reclamation and prevents dangling reference by using runtime assginment
  checks.

* __The operating system kernel layer__: We modified the Android customised
  Linux kernel to be compatible with Linux-RT patch, including real-time
  scheduling plocies, synchronisation primitives with priority awareness and
  basic syscalls for user spaces. Meanwhile, we enable the cross context
  communication between the stock Android and our real-time solution by using
  shared memory.

We have built a number of real-time applications on top of our solution
including a simulated cochlear implant application, a UAV control application,
an audio framework for device cooperation and distance estimation with sound
beckons.


More details can be found at [here](http://rtdroid.cse.buffalo.edu).
