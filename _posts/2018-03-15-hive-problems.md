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




# 编码问题
1. <b><font color='red'>中文乱码问题</font></b>

 <b>问题:</b>在视图中使用中文作为字段的值，在hive中运行会出现乱码(使用mysql做metadata的存储介质,postgres没有问题)。例如币种的合计。

 <b>解决方案:</b>ALTER DATABASE <db-name> CHARACTER SET utf8 COLLATE utf8_general_ci;<br/> 
refer to:[中文乱码](https://blog.csdn.net/jiacai2050/article/details/11782287)

如果此时，问题依然没有解决，查看mysql里面hive metastore数据库里的表：<b>TBLS</b>

```
show create table TBLS;
```
查看表的编码,如果编码不为UTF8,执行下述命令：<br/>

```
alter table TBLS convert to character set utf8 collate utf8_general_ci;
```

使用下述命令查看视图中是否还依然存在乱码，如果还存在，重新创建该视图：
```
select * from TBLS where TBL_NAME='视图的名字';
```

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
set hive.auto.convert.join=false;
```
在客户端设置这个参数，禁止自动将小表加载到内存。

   - method 2:更新统计信息
```
ANALYZE TABLE tablename

[PARTITION(partcol1[=val1], partcol2[=val2], ...)]

COMPUTE STATISTICS [noscan];
```

# hive相关的配置
1. 指定hive sql语句运行前为其指定资源池

```
SET mapreduce.job.queuename=［queune name］;

eg.
```
SET mapreduce.job.queuename=root.default;
select count(*) from tbl;
```

那么该sql就会在指定的pool:root.default中运行。

补充：也可以在连接hive的时候指定，例如：
```
jdbc:hive2//hadoop101:10000/detail;?mapreduce.job.queuename=root.default
```

# 参考
 - [hive中的mapjoin](https://blog.csdn.net/yycdaizi/article/details/50158573)
 - [hive 动态分区](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)
