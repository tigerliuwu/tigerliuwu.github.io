---
layout: post
title:  "信贷汇总统计分析 - apache kylin 读写分离集群安装"
categories: [kylin]
tags: [kylin]
author: Wu Liu
---

* content
{:toc}
基于已有的CDH5.11.2集群构建一个kylin读写分离的集群，该kylin集群跟CDH不在相同的机器上。<br>
同时，CDH集群上，确保HBASE集群的相应节点没有hive、spark、mapreduce等计算角色。




# 环境准备

将需要安装kylin的机器为：test1(all), test2(query)

## ssh 认证
在test1和test2上运行：<br>
```
ssh-copy-id cdh集群的各个节点hostname
```

## 将test1, test2 作为新集群节点加入到CDH中
 - step 1: 找到对应的CDH存储库，一般都在:$ /etc/yum.repos.d/cloudera-manager.repo 中可以找到对应的url。
 - step 2: 在CDH集群中的“所有主机”的右上角有个按钮：“向集群添加新主机”，添加test1, test2。直到“选择模板”为止。
 - step 3: 回到“所有主机”页面，首先将test1, test2 解除授权，再将它们“删除”。
<br>
达到的目的：test1， test2包含CDH所有客户端所需要的jars以及命令。（即拥有hadoop、hbase、hive命令）

## 拿到CDH集群所有的配置文件
```
$cd /etc/hadoop/conf
```
需要该目录下6个文件：core-site.xml, hdfs-site.xml, hbase-site.xml, hive-site.xml, mapred-site.xml, yarn-site.xml。
<br/>

将这些文件放到kylin_home的hadoop_conf文件夹下。

## 配置变量

### 配置kylin.properties
```
kylin.source.hive.client=beeline
kylin.source.hive.beeline-shell=beeline
kylin.source.hive.beeline-params=-u jdbc:hive2:hostname:10000 #hostname为hiveserver2所在的节点
kylin.query.max-return-rows=10000000  #最大返回结果条数
```

### 配置系统环境变量

 - 修改/etc/profile

```
KYLIN_HOME=/opt/apache-kylin-2.3.2-bin
HIVE_CONF=/opt/apache-kylin-2.3.2-bin/hadoop_conf/
HCAT_HOME=/opt/cloudera/parcels/CDH/lib/hive-hcatalog/share/hcatalog/
HIVE_LIB=/opt/cloudera/parcels/CDH/lib/hive/lib
HBASE_CONF_DIR=/opt/apache-kylin-2.3.2-bin/hadoop_conf/
HADOOP_CONF_DIR=/opt/apache-kylin-2.3.2-bin/hadoop_conf/
HBASE_CONF=/opt/apache-kylin-2.3.2-bin/hadoop_conf/
```
**执行$source /etc/profile**

# 运行kylin

## 启动和停止kylin

```
bin/kylin.sh start
bin/kylin.sh stop
```

日志打印出：**web ui is at http://<hostname>:7070/kylin**,表示成功安装(user/pwd:ADMIN/KYLIN)

**问题：**<br/>
<font color='red'>如果没有正确启动，根据显示的错误信息进行配置排查。例如：显示zookeeper无法连接，可以通过hbase shell来证明是否hbase客户端是否正常运行。如果没有正常运行，查看/etc/hbase/conf/hbase-site.xml文件，如果该文件为空，需将该文件替换掉。</font>

## metadata备份和恢复

```
bin/metastore.sh backup
bin/metastore.sh restore %path%
```

# 参考
 - [CDH追加节点](https://cloud.tencent.com/developer/article/1078286)
 - [kylin读写分离安装](http://www.bubuko.com/infodetail-2516040.html)
