---
layout: page
title: PhD Thesis
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

{% include image-row.html
  img1="pics/juav.jpg" caption1="Fig 1.1 Java-based Autopilot Control"
  img2="pics/turbine.jpg" caption2="Fig 1.2 Wind Turbine Monitor"
  img3="pics/ci.jpg" caption3="Fig 1.3 Cochlear Implant Simulation"
  main-caption="Fig.1 Targeting Applications"
%}


{% include image.html
            wrapper-class="image-wrapper400"
            img="pics/arch.jpg"
            title="RTDroid System Architecture"
            caption="Fig.4 RTDroid System Architecture" %}


___RTDroid___ presents a real-time extension on Android for real-time
applications, which require strict timing guarantees while still leveraging
existing functionalities on Android. More specifically, RTDroid provides a
real-time software stack separating from the original Android stack and
isolates interferences from non-real-time (Non-RT) apps to real-time (RT) apps
while still enabling interaction. Thereby, the development of real-time apps
can utilize existing functionalities of Android on RTDroid. RTDroid leverages a
real-time kernel, a real-time Java virtual machine and redesigned real-time
framework.

### Real-Time Kernel and Real-Time Java Virtual Machine

We ___partially___ patched the Android customised Linux kernel with Linux-RT
patch, including real-time scheduling policies, synchronisation primitives. We
enable priority awareness in task scheduling and locking as well as basic
syscalls for user spaces.


We have ported an existing real-time Java virtual machine, FijiVM, on
Android-based Linux kernel for the real-time runtime environment. It provides
basic real-time Java programming APIs for the development of the RTDroid
framework and has an Ahead-Of-Time (AOT) compiler for static compilation which
takes source codes of RTDroid and generates a native binary file.

### Real-Time Programming Model

{% include image.html
            img="pics/app-manifest.jpg"
            wrapper-class="image-wrapper400"
            title="RTDroid Application Manifest Code Snippet"
            caption="Fig.5 RTDroid Application Manifest Code Snippet" %}

RTDroid provides a redesigned real-time framework implementation for
application abstraction and communication APIs. It presents our solution as a
real-time programming model that extends Android's basic application components
for real-time development. Our programming model consists of real-time
components for application expressiveness, real-time channels for interaction
and real-time manifest schema for component declarations.

To prevent interferences from the runtime memory management, we alter the
traditional garbage collection to an on-stack memory management inspired by the
scoped memory of RTSJ. The on-stack memory management preserves memory bound
for object allocation for each component, which assures a constant cost for
memory reclamation and prevents any dangling reference by using runtime
assginment checks.

From a developer's point of view, our real-time components extend Android
components, and our real-time application manifest is similar to the Android
manifest for component declaration and real-time parameter configuration.
Thereby, the real-time parameters are used for scheduling validation and model
checking.


### Static Application Validation

{% include image.html
            img="pics/app-validation.jpg"
            wrapper-class="image-wrapper500"
            title="RTDroid Static Application Validation Workflow"
            caption="Fig.6 RTDroid Static Application Validation Workflow" %}

RTDroid integrates its application manifest with Cheddar*, a
real-time scheduling analysis tool, for static application validation. We
utilize real-time parameters configured in the RTDroid’s application manifest
and feed these parameters into Cheddar for feasibility analysis, including
schedulability, memory usage and communication channel overflow.

---

More details can be found at [RTDroid website](http://rtdroid.cse.buffalo.edu)
and the full version of my thesis will be available soon.


\* For more information about [Cheddar](http://beru.univ-brest.fr/~singhoff/cheddar/).

