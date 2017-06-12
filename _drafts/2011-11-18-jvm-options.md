---
layout: post
title: Demystify command-line options of Java Virtual Machine
author: gassa
category: Others
tags: [Java, JVM]
---

other blog:
https://10kloc.wordpress.com/2013/05/10/command-line-options-in-java-virtual-machine/


the spec of java language &  the spec of jvm
https://docs.oracle.com/javase/specs/


Oracle JVM command options:
http://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html

http://community.jaspersoft.com/wiki/sun-openjdk-jvm-garbage-collection-tuning

https://10kloc.wordpress.com/2013/05/10/command-line-options-in-java-virtual-machine/

```Shell
https://docs.oracle.com/cd/E13150_01/jrockit_jvm/jrockit/jrdocs/refman/

java -X  #list of -X options for JVM runtime.


java -XshowSettings:vm -Xlog:gc:PrintGCDetails -Xmx64m -Xms64m Fibonacci 1000000
```
