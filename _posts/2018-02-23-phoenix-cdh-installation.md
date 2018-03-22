---
layout: post
title:  "phoenix - 基于CDH安装"
categories: [phoenix]
tags:  [big data, phoenix]
author: Wu Liu
---

* content
{:toc}
基于CDH5.11.2安装phoenix.
hbase master: cdhdev178(active),cdhdev180(backup)<br/>
hbase regionserver:cdhdev181,cdhdev182,cdhdev183,cdhdev184<br/>
zookeeper: cdhdev182(leader),cdhdev181(follow),cdhdev180(follow) port:2181<br/>




# 1.下载和安装phoenix

## 1.1 下载
下载跟CDH对应的phoenix安装包，这里采用的是CDH5.11.2的hadoop平台，下载对应的[cdh phonenix](http://mirror.bit.edu.cn/apache/phoenix/apache-phoenix-4.13.2-cdh5.11.2/bin/)

## 1.2 安装
 - 解压缩
 - 将其中的phoenix-server.jar放到每个region server对应的hbase lib目录下（/opt/cloudera/parcels/CDH/lib/hbase/lib）
 - restart HBASE

## 1.3 客户端sqlline.py
进入到phoenix的bin目录下，运行下述语句：

```
./sqlline.py cdhdev180,cdhdev181,cdhdev182:2181
```

use the following command to check if it works well
```
!tables
```

## 1.4 配置客户端squirrel
 - jars:phoenix-client.jar
 - url:<br/>
    jdbc:phoenix:cdhdev180,cdhdev181,cdhdev182:2181
 - jdbc driver: org.apache.phoenix.jdbc.PhoenixDriver
# 2.事务性(transaction)

## 2.1 配置(configuration)

 - hbase client:hbase-site.xml

```
<property>
  <name>phoenix.transactions.enabled</name>
  <value>true</value>
</property>
```

 - hbase server:hbase-site.xml

 **This part is used to configure the transaction manager:**
 **The "Transaction server configuration" section of Tephra**

```
<property>
  <name>data.tx.snapshot.dir</name>
  <value>/tmp/tephra/snapshots</value>
</property>
```

 **set the transaction timeout.**
```
<property>
  <name>data.tx.timeout</name>
  <value>60</value>
</property>
```

 - set $HBASE_HOME and start the transaction manager:
```
./bin/tephra
```

# reference
 - [phoenix quick start](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html)
 - [phoenix transaction](http://phoenix.apache.org/transactions.html#)
 - [phoenix for CDH](https://www.cnblogs.com/zlslch/p/7096402.html)
 - [phoenix 技术分享](http://blog.csdn.net/silentwolfyh/article/details/70208954)
