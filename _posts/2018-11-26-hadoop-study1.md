---
layout: post
title:  "CDH - study系列"
categories: [hadoop]
tags: [hadoop]
author: Wu Liu
---

* content
{:toc}




# hive on spark

## 原理
1. spark on yarn cluster

2. spark on yarn client

[Spark on Yarn的运行原理](https://blog.csdn.net/u013573813/article/details/69831344)

## 调优


refer to:
 - [Hive on Spark调优](http://linbingdong.com/2016/11/30/Hive%20on%20Spark%E8%B0%83%E4%BC%98/)




# 遗留的问题
1. 如何通过YARN控制为不同的SPARK集群管理资源？

2. 如何控制hdfs, hive的访问权限？

3. 如何结合硬件资源来确定hdfs的总容量？

4. 测试集群的性能的办法
 - TPCx-BB


# 参考
 - [hive中的mapjoin](https://blog.csdn.net/yycdaizi/article/details/50158573)
 - [hive 动态分区](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)
