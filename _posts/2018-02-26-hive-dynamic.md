---
layout: post
title:  "hive - 动态分区"
categories: [hive]
tags: [hive]
author: Wu Liu
---

* content
{:toc}
介绍动态分区的创建和使用。





# 动态分区表的创建
```
create table partition_test(
id int,
name string
)partitioned by (
stat_date date)
row format delimited fields terminated by ',';
```

# 往动态表里插入数据

## examples

在hive客户端设置一下参数：
```
set hive.exec.dynamic.partition=true;
set hive.exec.dynamic.partition.mode=nonstrict;
set hive.exec.max.dynamic.partitions.pernode=2000; --default 1000
set hive.exec.max.dynamic.partitions=10000;
--set optimize.sort.dynamic.partition=true;
```

然后在客户端运行插入语句：
```
insert overwrite table partition_test partition (stat_date) select id1,name1, to_date(dsr_insert_time) from normal_test;
```

## QA

1. 如果发生：
```
caused by:java.io.EOFException:Premature EOF: no length prefix available
```

 - 方法一:使用参数
设置：

```
set optimize.sort.dynamic.partition=true;
```
 - 方法二：使用distribute by 分区字段
```
insert overwrite table partition_test partition (stat_date) select id1,name1, to_date(dsr_insert_time) from normal_test distribute by to_date(dsr_insert_time);
```

即可解决。

## 调试方法

[reduce端缓存数据过多出现FGC，导致reduce生成的数据无法写到hdfs](http://www.cnblogs.com/hark0623/p/4196378.html)

# 参考
 - [hive 动态分区](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)
