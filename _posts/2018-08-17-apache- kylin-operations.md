---
layout: post
title:  "信贷汇总统计分析 - apache kylin操作"
categories: [kylin]
tags: [kylin]
author: Wu Liu
---

* content
{:toc}
汇总kylin2.3.1操作方式




# work with kylin

## 删除kylin中所有的cubes
 - 启动hbase shell

```
hbase shell
```

 - 使用命令disable_all 和 drop_all来删除这些表

dsiable所有cubes 对应的表:<br/>
```
disable_all 'KYLIN_.*'
``` 

删除所有的表：<br/>
```
drop_all 'KYLIN_.*'
```

 - 删除kylin所有元数据所在的表

```
disable_all 'kylin.*'

drop_all 'kylin.*'
```

**此刻重新启动kylin，就会发现所有的project信息、model、cubes信息都清理完成，但是hdfs对应的/kylin目录下依然保留历史数据**

# 参考
 - [apache kylin](http://www.cnblogs.com/dreamfactory/p/5588203.html)
