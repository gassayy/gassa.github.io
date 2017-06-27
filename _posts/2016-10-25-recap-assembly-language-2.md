---
layout: post
title: Recap Assembly Language 2
author: gassa
---
For example, if we convert an executable binary file to its text format with 
`objdump -s xxx.exe`, we then will get the Executable and Linkable Format (ELF) 
of it. The ELF file contains **symbols**, eg. function names, global variables,
and etc., these symbols are orginzed into **sections** --- code lives in one 
section (.text) and data in another (.data, rodata), then these sections are 
organized into **segments** or program headers. Addition to `objdump`, there 
are also other tools that can help us to explore the ELF file. Like, 
`readelf --segments exe` and `nm exe`. the `readelf` shows how an executable 
are segmented and loaded into memory. I don\'t want to include too much 
details here, instead I will summarize them in my next blog.


