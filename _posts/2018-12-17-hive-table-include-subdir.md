---
layout: post
title:  "CDH - hive表支持子目录作为输入"
categories: [hadoop, hive]
tags: [hadoop, hive]
author: Wu Liu
---

* content
{:toc}
通过设置hive参数，使hive表支持子目录作为输入




# mapreduce

默认情况下，
```
mapreduce.input.fileinputformat.input.dir.recursive=false;
```

 - 现象

在这种情况下，如果数据源中包含**子目录**，那么会将该子目录当作文件进行处理，从而抛出文件不存在的异常信息。

 - 解决

将该参数改为true后，mapreduce程序可以正常执行。可在mapreduce程序里面设置，也可在mapred-site.xml设置


# hive
默认情况下：
```
hive.mapred.supports.subdirectories=false;
mapreduce.input.fileinputformat.input.dir.recursive=false;
```

同 **mapreduce** 一样，如果包含子目录，该子目录会被当成文件进行处理，从而抛出文件不存在的异常信息。

```
set hive.mapred.supports.subdirectories=true;
mapreduce.input.fileinputformat.input.dir.recursive=true;
```



# 参考
 - [MapReduce和Hive支持递归子目录作为输入](http://lxw1234.com/archives/2015/07/382.htm)
