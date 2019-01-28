---
layout: post
title:  "kylin - installation"
categories: [kylin]
tags: [kylin]
author: Wu Liu
---

* content
{:toc}
kylin安装相关的细节




# 介绍
在集群里面使用命令：
```
beeline -h
```
即可得到beeline的所有参数，其中-n 用来指定用户名，而-u用来指定jdbc的url。

# 用法
```
beeline -n root -u jdbc:hive2://hadoop-srv-2:10000 --color=true --silent=true --outputformat=csv2 --showHeader=false -f filename.sql
```

具体有哪些参数以及使用方式，可以使用beeline -h来查看。

# 参考
