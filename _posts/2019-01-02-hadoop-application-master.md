---
layout: post
title:  "CDH - application master介绍"
categories: [hadoop, hive]
tags: [hadoop, hive]
author: Wu Liu
---

* content
{:toc}
介绍application master的作用以及发现的问题。
ApplicationMaster会为该作业所有的map和reduce任务向ResourceManager请求容器（包括内存资源和CPU资源）；附着心跳信息的请求包括map任务的本地化信息，如输入分片所在的主机和机架信息。ResourceManager根据这些信息完成分配决策，理想情况会将任务分配给数据本地化的节点。





# 问题
## 问题1：Yarn Application Master堆内过小
 - 现象： 这个情况的实际现象就是Map任务都处理成功，然而yarn任务一直在RUNNING状态，过了一会就出现OOM。通过查看系统默认的Application Master堆内存大小仅为800多MB，但此次任务有9万多个Map任务，所以YARN在异步处理信息的时候明显的内存资源不足就会出现失败。

 - 解决方案
```
yarn.app.mapreduce.am.resource.mb=8192  #默认是50
yarn.app.mapreduce.am.command-opts =-Xmx6144m  #默认是10000
```
上述两个参数的建议值为：
```
yarn.app.mapreduce.am.resource.mb= 2 * RAM-per-container
yarn.app.mapreduce.am.command-opts= 0.8 * 2 * RAM-per-container
```


# 参考
 - [YARN框架](https://www.cnblogs.com/wangrd/p/6135954.html)
 - [Hadoop/Yarn/MapReduce内存分配（配置）方案](https://blog.csdn.net/bluishglc/article/details/42436321)
