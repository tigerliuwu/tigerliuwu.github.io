---
layout: post
title:  "信贷汇总统计分析 - apache kylin问题汇总"
categories: [kylin]
tags: [kylin]
author: Wu Liu
---

* content
{:toc}
汇总kylin2.3.0中碰到的所有问题




# work with kap

## 安装
 - kap 和 kylin使用相同jdbc url
 - user/pwd：ADMIN/KYLIN
 - env 命令结合grep的使用方法，可以用来查看环境变量

## 备份
 - 使用sh 命令
```
metastore.sh -h
```

即可查看备份cube的metadata所需的所有命令。

  - 

## other cmd

 - 从文件中查找对应的字符串
```
grep -i text file
```

 - 动态查看文件（例如日志在不断更新的时候）
```
tail -f file
```


# problems

## build cube
1. <b>在step3，extract fact table distinct columns or step 4</b><br/>

<b>抛出异常信息</b>：the value of property statistic.enabled must not be null.

<b>解决方案:</b>

2. <b><font color='red'>构建cube的第一步失败</font></b>

step 1 name:create intermediate flat hive table的时候，抛出NPE, 查看日志信息，发现在mysql里面对应step 1的hive 宽表 没有创建出来。

<b>解决方案:</b>
可能是HCatalog安装有点问题。也有可能kap RC版不稳定。

## query
1. fact表和维度表关联的时候，使用组合主键。那么该维度表的derived字段的值在cube里面都是空的。

2. 集合A union 集合B，如果A为空，那么基于A union B作聚合计算会抛错



# 解决的问题

1. 内存溢出
```
Java Heap Exception
```

我们每次启动kylin，其实只是读取kylin.properties里面的参数，应用于hbase，并启动tomcat。<br/>
在tomcat/bin目录下添加setenv.sh，在其中添加如下参数：
```
CATALINA_OPTS="
-server 
-Xms6000M 
-Xmx6000M 
-Xss512k 
-XX:NewSize=2250M 
-XX:MaxNewSize=2250M 
-XX:PermSize=128M
-XX:MaxPermSize=256M
"
```

refer to :[tomcat jvm](https://blog.csdn.net/ldx891113/article/details/51735171)

2. 启动kylin的时候，报出kylin_cluster_meta表已经存在的错误

解决步骤：通过Hbase shell命令进入hbase客户端，list所有表名，发现根本没有kylin_cluster_meta表。<br/>
难道表在.meta里面没有被发现？采用scan 'hbase:meta'命令，打印出来的信息依然没有任何相关信息。<br/>
那只能说明在hdfs的某个地方存放着hbase表的信息，造成建表的时候，落地到hdfs的时候发现该地址已经被占用了。


最终解决方案：<br/>
```
find /opt -name 'zkcli.sh'
```

找到对应的zkcli.sh文件，根据其参数得到下属命令：
```
sh zkcli.sh -zkhost:zkhost1:2181,zkhost2:2181,zkhost3:2181 -cmd clear /kylin
```

在此之前，可以通过下属命令查看/kylin目录下面都有哪些信息，如下：<br/>
```
sh zkcli.sh -zkhost:zkhost1:2181,zkhost2:2181,zkhost3:2181 -cmd ls /kylin
```
可以从里面看到所有kylin meta相关的信息。<br/>

3. 在查询的时候会抛出：
```
org.apache.kylin.rest.exception.InternalErrorException:Query returned xxxxxxxxx rows exceeds threshold 5000000
```

<b>solution:</b>

在kylin.properties里面设置
```
kylin.query.max-return-rows=10000000
```









# 参考
 - [apache kylin](http://www.cnblogs.com/dreamfactory/p/5588203.html)
