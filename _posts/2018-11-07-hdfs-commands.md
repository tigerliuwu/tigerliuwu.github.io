---
layout: post
title:  "hdfs - 常用命令"
categories: [hdfs]
tags: [hdfs]
author: Wu Liu
---

* content
{:toc}
hdfs常用命令。




# 查看命令
 - 查看路径
```
hadoop fs -ls path
```

 - 查看路径下所有文件和子目录的大小
```
hadoop fs -du -h -s path
```

-----------------------------------------------------------

# 修改命令

 - 上传本地文件到hdfs
```
hadoop fs -put hdfs_path local_path
```

 - 下载到本地文件
```
hadoop fs -get 
```

# 维护命令

 - 查看hdfs中文件路径以及大小
```
hadoop fs -du -h  /user/.../..
```

 - 清空hdfs回收站
```
hadoop fs -expunge
```


# 参考
 - [hive中的mapjoin](https://blog.csdn.net/yycdaizi/article/details/50158573)
 - [hive 动态分区](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)
