---
layout: post
title:  "hadoop - sqoop"
categories: [sqoop]
tags: [sqoop]
author: Wu Liu
---

* content
{:toc}
sqoop使用介绍





# sqoop简介
 sqoop 命令在执行的过程中，会生成mapreduce代码，会在sqoop命令的执行路径下生成一个java文件，该文件为mapreduce文件，可以用来查看sqoop执行逻辑。<br>
其有两个主要命令如下，此处主要介绍import：

 - import
 将关系数据库中的表数据导入到HDFS/Hive/HBASE等

 - export
 将hadoop的HDFS/Hive/HBASE等中的数据导出到关系数据库

# 配置
 - 将对应的关系数据库的驱动包例如：oracle的驱动包：ojdbc7.jar放到sqoop client所在的服务器上，路径为：../CDH/lib/sqoop/lib/

# export命令
```
sqoop export \
  --hcatalog-database temp \
  --hcatalog-table js_pianyuan_orc \
  --hcatalog-partition-keys curdate \
  --hcatalog-partition-values 20180122 \
  --connect jdbc:mysql://ip:3306/test \
  --username username \
  --password passwd --m 10\
  --table js_pianyuan
```

refer to: [hive中orc表sqoop导出到mysql](https://www.jianshu.com/p/e6c6bfef33bc)

# import命令

  - 参数<b>--hive-drop-import-delims</b>会将oracle数据库表中特殊字符：\n \r \001 去掉，从而解决数据源脏乱的问题。<br/>
例如：如果库表中包括换行符\n，而sqoop默认换行符为导出的hdfs文件的行的分隔符，那么oracle的一条记录，在hdfs就是两行记录。

  - 参数<b>--hive-overwrite</b>，指定该参数，那么目标hive表将会被覆盖；不指定，则对hive目标表进行追加操作。

  - 参数<b>--split-by columns</b>,最好选择数据分布比较均匀的columns，避免部分mapper instance空跑，而一些负载过重。跟参数-m num配合使用。

## 导入到HDFS

```
sqoop import connect jdbc:oracle:thin:@localhost:1521:db --username user --password pwd --table oracle_table --target-dir /user/.../hdfs_dir --split-by column_names -m num_mappers --hive-drop-import-delims --field-terminated-by '\001' --append
```

## 导入到HIVE表

 - 无分区的方式
```
sqoop import connect jdbc:oracle:thin:@localhost:1521:db --username user --password pwd --table oracle_table --hive-import --hive-overwrite --hive-table hivedb.hivetable --hive-drop-import-delims --field-terminated-by '\001'
```

 - 有分区的方式（单分区）&orc格式
```
sqoop import connect jdbc:oracle:thin:@localhost:1521:db --username user --password pwd --table oracle_table --hcatalog-database odsa --hcatalog-table odsa_table --hive-drop-import-delims --field-terminated-by '\001' -m 1 --hive-partition-key rdate --hive-partition-value '20180909' --hcatalog-storage-stanza 'stored as orcfile'
```

 - 有分区的方式（多分区）&orc格式
```
sqoop import connect jdbc:oracle:thin:@localhost:1521:db --username user --password pwd --table oracle_table --hcatalog-database odsa --hcatalog-table odsa_table --hive-drop-import-delims --field-terminated-by '\001' -m 1 --hcatalog-partition-keys rdate,rtime --hcatalog-partition-value '20180909','120101' --hcatalog-storage-stanza 'stored as orcfile tblproperties ("orc.compress"="SNAPPY")'
```

## 导入数据格式为csv

<b>如果库表里有些字段包括列分隔符，可以将数据导出为csv文件来解决；但是如果包含一些回车符，那么必须通过参数：参数<b>--hive-drop-import-delims</b> 来删除数据源中脏数据的无用字符。</b>

### 用法和用例

 - 用法
```
sqoop import connect jdbc:oracle:thin:@localhost:1521:db --username user --password pwd --table oracle_table --hive-import --hive-overwrite --hive-table hivedb.hivetable --hive-drop-import-delims --field-terminated-by ',' --escaped-by `\\` --enclosed-by '\"'
```

导出来的文件结果如下：<br/>
使用命令：<br/>
```
hadoof fs -tail /user/..../hadoop-map-001
```
查看得到的文件如下：<br/>
```
"aaa","\"nihao","null","30"
"bbb","world","null","50"
```

<b>如上所示，关系表中的null被转为文件中的"null".可使用下属变量将关系表中的null转为自己想要的值，例如：</b>
```
--null-string '\\N'
--null-nong-string '\\N'
```


 - 对应的hive设置

```
CREATE TABLE my_table(a string, b string, ...)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
   "separatorChar" = ',',
   "quoteChar"     = '\"',
   "escapeChar"    = '\\'
)  
STORED AS TEXTFILE;
```

同时，在设置serdeproperties的一个属性如下：
```
alter table ${table_name} set serdeproperties('serialization.null.format'='\\N')
```

### 问题

#### 问题1
 - 现象：Caused by:java.sql.SQLRecoverableException:IO Error: Connection reset<br/>
 - 原因：Sqoop 2 Server的Java堆栈(Java Heap Size of Sqoop 2 Server in Bytes)设置太少，
 - 解决：调大即可。
 - refer to [Java Heap Size of Sqoop 2 Server in Bytes](https://www.cloudera.com/documentation/enterprise/properties/5-8-x/topics/cm_props_cdh470_sqoop2.html)

#### 问题2
 - <font color='red'>oracle表中null值，导出的文件格式为csv时，空值无法识别。</font>

#### 问题3

 - <b>Problem: When using the default Sqoop connector for Oracle, some data does get transferred, but during the map-reduce job a lot of errors are reported as belows:</b>
```
11/05/26 16:23:47 INFO mapred.JobClient: Task Id : attempt_201105261333_0002_m_000002_0, Status : FAILED
java.lang.RuntimeException: java.lang.RuntimeException: java.sql.SQLRecoverableException: IO Error: Connection reset
at com.cloudera.sqoop.mapreduce.db.DBInputFormat.setConf(DBInputFormat.java:164)
at org.apache.hadoop.util.ReflectionUtils.setConf(ReflectionUtils.java:62)
at org.apache.hadoop.util.ReflectionUtils.newInstance(ReflectionUtils.java:117)
at org.apache.hadoop.mapred.MapTask.runNewMapper(MapTask.java:605)
at org.apache.hadoop.mapred.MapTask.run(MapTask.java:322)
at org.apache.hadoop.mapred.Child$4.run(Child.java:268)
at java.security.AccessController.doPrivileged(Native Method)
at javax.security.auth.Subject.doAs(Subject.java:396)
at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1115)
at org.apache.hadoop.mapred.Child.main(Child.java:262)
Caused by: java.lang.RuntimeException: java.sql.SQLRecoverableException: IO Error: Connection reset
at com.cloudera.sqoop.mapreduce.db.DBInputFormat.getConnection(DBInputFormat.java:190)
at com.cloudera.sqoop.mapreduce.db.DBInputFormat.setConf(DBInputFormat.java:159)
... 9 more
Caused by: java.sql.SQLRecoverableException: IO Error: Connection reset
at oracle.jdbc.driver.T4CConnection.logon(T4CConnection.java:428)
at oracle.jdbc.driver.PhysicalConnection.<init>(PhysicalConnection.java:536)
at oracle.jdbc.driver.T4CConnection.<init>(T4CConnection.java:228)
at oracle.jdbc.driver.T4CDriverExtension.getConnection(T4CDriverExtension.java:32)
at oracle.jdbc.driver.OracleDriver.connect(OracleDriver.java:521)
at java.sql.DriverManager.getConnection(DriverManager.java:582)
at java.sql.DriverManager.getConnection(DriverManager.java:185)
at com.cloudera.sqoop.mapreduce.db.DBConfiguration.getConnection(DBConfiguration.java:152)
at com.cloudera.sqoop.mapreduce.db.DBInputFormat.getConnection(DBInputFormat.java:184)
... 10 more
Caused by: java.net.SocketException: Connection reset
at java.net.SocketOutputStream.socketWrite(SocketOutputStream.java:96)
at java.net.SocketOutputStream.write(SocketOutputStream.java:136)
at oracle.net.ns.DataPacket.send(DataPacket.java:199)
at oracle.net.ns.NetOutputStream.flush(NetOutputStream.java:211)
at oracle.net.ns.NetInputStream.getNextPacket(NetInputStream.java:227)
at oracle.net.ns.NetInputStream.read(NetInputStream.java:175)
at oracle.net.ns.NetInputStream.read(NetInputStream.java:100)
at oracle.net.ns.NetInputStream.read(NetInputStream.java:85)
at oracle.jdbc.driver.T4CSocketInputStreamWrapper.readNextPacket(T4CSocketInputStreamWrapper.java:123)
at oracle.jdbc.driver.T4CSocketInputStreamWrapper.read(T4CSocketInputStreamWrapper.java:79)
at oracle.jdbc.driver.T4CMAREngine.unmarshalUB1(T4CMAREngine.java:1122)
at oracle.jdbc.driver.T4CMAREngine.unmarshalSB1(T4CMAREngine.java:1099)
at oracle.jdbc.driver.T4CTTIfun.receive(T4CTTIfun.java:288)
at oracle.jdbc.driver.T4CTTIfun.doRPC(T4CTTIfun.java:191)
at oracle.jdbc.driver.T4CTTIoauthenticate.doOAUTH(T4CTTIoauthenticate.java:366)
at oracle.jdbc.driver.T4CTTIoauthenticate.doOAUTH(T4CTTIoauthenticate.java:752)
at oracle.jdbc.driver.T4CConnection.logon(T4CConnection.java:366)
... 18 more
```

 - Solution: This problem occurs primarily due to the lack of a fast random number generation device on the host where the map tasks execute. On typical Linux systems this can be addressed by setting the following property in the java.security file:

```
securerandom.source=file:/dev/../dev/urandom
```


[Oracle: Connection Reset Errors](https://sqoop.apache.org/docs/1.4.6/SqoopUserGuide.html#_oracle_connection_reset_errors)


# 参考
 - [hive open csv serde2](https://cwiki.apache.org/confluence/display/Hive/CSV+Serde)
 - [hive csv serde](https://github.com/ogrodnek/csv-serde)
 - [sqoop参数hive-drop-import-delims](http://www.cnblogs.com/hark0623/p/5680754.html)
 - [通过Sqoop向Hive导入ORC表(多分区)](http://hejunhao.me/archives/628)
