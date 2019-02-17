---
layout: post
title:  "mysql - installation"
categories: [mysql]
tags: [mysql]
author: Wu Liu
---

* content
{:toc}
mysql安装相关的细节




# 安装

 - [下载](http://dev.mysql.com/downloads/mysql/5.6.html#downloads)

 - 解压

```
#解压
tar -zxvf mysql-5.6.33-linux-glibc2.5-x86_64.tar.gz
#复制解压后的mysql目录
cp -r mysql-5.6.33-linux-glibc2.5-x86_64 /usr/local/mysql
```
 - 添加用户组和用户(optional)
```
#添加用户组
groupadd mysql
#添加用户mysql 到用户组mysql
useradd -g mysql mysql
```

 - 安装
```
cd /usr/local/mysql/<br>mkdir ./data/mysql
chown -R mysql:mysql ./
./scripts/mysql_install_db --user=mysql --datadir=/usr/local/mysql/data/mysql
cp support-files/mysql.server /etc/init.d/mysqld
chmod 755 /etc/init.d/mysqld
cp support-files/my-default.cnf /etc/my.cnf
 
#修改启动脚本
vi /etc/init.d/mysqld
 
#修改项：
basedir=/usr/local/mysql/
datadir=/usr/local/mysql/data/mysql
 
#启动服务
service mysqld start
 
#测试连接
./mysql/bin/mysql -uroot
 
#加入环境变量，编辑 /etc/profile，这样可以在任何地方用mysql命令了
export PATH=$PATH:/usr/local/mysql//bin<br>source /etc/profile
 
 
#启动mysql
service mysqld start
#关闭mysql
service mysqld stop
#查看运行状态
service mysqld status
```

# 登录和授权

 - cmd：
```
mysql -u <username> -p

CREATE DATABASE kyligence_metastore;
CREATE USER kyligence@'%' IDENTIFIED BY 'kyligence#2018';
GRANT ALL PRIVILEGES ON kyligence_metastore.* TO kyligence@'%' IDENTIFIED BY 'kyligence#2018';
FLUSH PRIVILEGES;
```

# 参考
 - [mysql在linux下的安装](https://www.cnblogs.com/bookwed/p/5896619.html)

