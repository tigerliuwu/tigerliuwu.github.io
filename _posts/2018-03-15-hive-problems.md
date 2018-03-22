---
layout: post
title:  "hive - 问题汇总"
categories: [hive]
tags: [hive]
author: Wu Liu
---

* content
{:toc}
hive采用spark作为计算引擎碰到的问题。





# sql问题

1. **两个大表关联操作发生将其中一个大表进行拆分进行mapjoin**
 - 原因分析：

查看执行计划

```
explain select ....
```

发现执行计划里面，将其中一个大表判断为只有100多条的数据，大小只有17k左右，结果hive引擎认为这是一张小表，按照小表优化该query，
在map端做mapside join，然后系统就按需分配一个内存用于缓存该数据，即40M（<font color='red'>为什么是40M，最小吗？</font>）。
但是在实际运行的时候，由于hive底层采用的spark抽取数据，发现40M的内存用完了，首先会将该数据块分发到各个计算结点
（这块非常可怕，如果参与计算的是所有的机器，那么会给所有机器分发一份数据）；
然后发现数据源依然有数据，那么就会继续申请内存用来缓存数据；这样就会造成内存消耗过大，会出现：
```
caused by:java.io.FileNotFoundException:File does not exist:/tmp/hive/liuwu/2d...../hive_<%date%>/-mr-/HashTable-stage-1/MapJoin
-mapfile--.hashfile/Hashtablesink_21-1436677929
```
 - 解决方案
   - method 1: refer to [map join](http://blog.csdn.net/woshixuye/article/details/53696257)
```
set hive.auto.convert.join=true;
```
在客户端设置这个参数，禁止自动将小表加载到内存。
   - method 2:更新统计信息
```
ANALYZE TABLE tablename

[PARTITION(partcol1[=val1], partcol2[=val2], ...)]

COMPUTE STATISTICS [noscan];
```


# 参考
 - [hive 动态分区](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)
