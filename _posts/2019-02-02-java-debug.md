---
layout: post
title:  "java - 调试监控工具"
categories: [java]
tags: [java]
author: Wu Liu
---

* content
{:toc}
java提供了很多强大的工具来协助查看java进程的工作状态，如jmap, jstack, jhat等,
这些工具对hadoop平台的运行状态具有很好的指导意义。




# 介绍

## jmap
 - jmap -heap %pid%

 - jmap -dump:live,format=b,file=heap.bin %pid%

## jstat
[java命令--jstat 工具使用](https://www.cnblogs.com/baihuitestsoftware/articles/6408381.html)

# 参考
 - [jmap和jstack使用](https://blog.csdn.net/sinat_29581293/article/details/70214436)
 - [JAVA的内存模型及结构](http://ifeve.com/under-the-hood-runtime-data-areas-javas-memory-model/)
