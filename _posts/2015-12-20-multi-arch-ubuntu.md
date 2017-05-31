---
layout: post
title:  How to Enable MultiArch on Ubuntu
author: gassa
category: Notes
tags: [ubuntu, command]
---

Multiarch is a term used to refer to the capability of a system to install and
run applications of multiple different binary targets on a same system. For
example, running a i386-linux-gnu application on an amd64-linux-gnu system. It
also simplifies cross-building, where foreign-architecture libraries and
headers are needed on the system during linking.

It is avaiable since Ubuntu 14.04. After we install system, the default
archtitecture will likely be amd64 as long as the machine is not an antique.

To check the local architecture, `dpkg --print-architecture`.

To add a new architectur, e.g., i386, `dpkg --add-architecture i386`.

To check list foreign architectures, `dpkg --print-foreign-architectures`.

To remove an architecture, `dpkg --remove-architecture i386`.

The package manager (apt) is multiarch-aware, it supports `apt-get install
package:architecture`. To install build-dependencies of a package before
cross-build, `apt-get build-dep -a <arch> <package>`.

For example, using the Android SDK requires 32-bit packages. There was a heck
to get 32-bits packages by using `apt-get install ia32-libs`. It install all
ia32 packages and has been replaced by MultiArch since Ubuntu 14.04.

```shell
dpkg --add-architecture i386
apt-get update
apt-get install libstdc++6:i386 libgcc1:i386 \
                zlib1g:i386 libncurses5:i386
```





