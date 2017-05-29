---
layout: page
title: About me
---

![yinyan-pic]({{ site.url }}/downloads/blog2.jpg)


I am Yin Yan (阎寅), a PhD. student of Computer Science and Engineering
Department at SUNY Buffalo. My research interest lies in operating system
design and programming language. My PhD. reserch is about to enable real-time
capability on Android platform, named
[RTDroid](http://rtdroid.cse.buffalo.edu).

RTDroid explores and presents our solution in each layer of the stock Android
software stack as the following list:

* __The application framework layer__: We add new abstraction for real-time
  application, including real-time components and real-time application
  manifest. Our real-time components extend Android components for application
  development, the real-time application manifest is similar to the Android
  manifest for component declaration and real-time parameter configuration.
  Thereby, the real-time parameters are used for scheduling validation and
  model checking.

* __The runtime layer__: We have ported an existing real-time Java virtual machine, FijiVM, on Android-based Linux kernel for the real-time runtime environment. It has an Ahead-Of-Time compiler that takes Java bytecodes of a real-time application and application manifest file, runs scheduling analysis and generates a binary executable for the targeting device.

* __The operating system kernel layer__: We modified the Android customised
  Linux kernel compatible for Linux-RT patch, including a real-time scheduler,
  synchronisation primitives with priority awareness and basic syscalls for
  user spaces. Meanwhile, we enable the cross context communication between the
  stock Android and our real-time solution by using shared memory.

We have built a number of real-time applications on top of our solution
including a simulated cochlear implant application, a UAV control application,
an audio framework for device cooperation and distance estimation with sound
beckons.
