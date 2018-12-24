---
layout: post
title:  "hdfs - 问题汇总"
categories: [hdfs]
tags: [hdfs]
author: Wu Liu
---

* content
{:toc}
hdfs使用中碰到的问题。




# 配置问题


-----------------------------------------------------------

# 运行问题

 1. hdfs中.trash文件很多，占用空间巨大（51T）<br/>

**问题：**<br/>
```
hadoop fs -du -h
```
可查看所有当前登陆用户在hdfs:/user/%loginuser%目录下所有子目录占用资源的情况，可发现.trash子目录占用了51T的hdfs空间。

**解决办法:**<br/>
  - 快速解决办法如下，将loginuser目录下.trash子目录进行清空：<br/>
```
$hadoop fs -expunge
```

  - 根据业务需要不需要保留该目录（开发测试才需要保留该目录）,即使用**fs.trash.interval**和**fs.trash.checkpoint.interval**<br/>
参考[Hadoop Trash回收站使用指南](https://blog.csdn.net/sunnyyoona/article/details/78869778)

 2. hdfs文件duplicates数量设置和blocksize设置<br/>
在hdfs-site.xml文件中进行设置。
```
dfs.replication=3  # 数据块副本数。
dfs.replication.max=512 # 最大块副本数，不要大于节点总数。
dfs.namenode.replication.min=1 # 最小块副本数。在上传文件时，达到最小副本数，就认为上传是成功的
```

```
dfs.blocksize=67108864   # 块大小，字节。可以使用后缀: k(kilo), m(mega), g(giga), t(tera), p(peta), e(exa)指定大小 (就像128k等)。
```

 3. hdfs容量大小的设置和**硬件的配置**(unresolved)
  - 每个磁盘（volume）的保留空间，字节。要注意留足够的空间给非HDFS文件使用。建议保留磁盘容量的5%或者50G以上
```
dfs.datanode.du.reserved=0 
```
  - hdfs文件存储地址
```
dfs.datanode.data.dir=file://${hadoop.tmp.dir}/dfs/data
```

# 参考
 - [hadoop参数设置](https://www.cnblogs.com/peizhe123/p/5540845.html)
