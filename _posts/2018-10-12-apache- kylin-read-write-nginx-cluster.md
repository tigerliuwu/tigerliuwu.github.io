---
layout: post
title:  "信贷汇总统计分析 - apache kylin 读写分离集群安装-nginx"
categories: [kylin]
tags: [kylin]
author: Wu Liu
---

* content
{:toc}
使用nginx作为kylin集群的负载均衡器。




# 环境准备

**前置安装:**
 - 下载pcre、zlib、openssl tar.gz的源代码包
 - 解压缩以及安装
```
tar -zxvf source.tar.gz
cd %project%
./configure
make
make install
```

# 安装nginx
```
tar -zxvf nginx.tar.gz
cd nginx
./configure --prefix=/usr/local/nginx \
--with-pcre=%pcre directory% --with-pcre=%zlib directory% --with-pcre=%openssl directory%

make
make install
```

**nginx就会安装到/usr/local/nginx目录下**

# 配置nginx
在安装目录下的：conf/nginx.conf中修改：
```
upstream tomcats{
server hadoop105:7070 weight=8;
server hadoop104:7070 weight=8;
}

server {
listen 28080; #端口号
server_name localhost;
location / {
proxy_pass http://tomcats;
}

# 运行nginx
```
sbin/nginx   #启动
sbin/nginx -s stop #停止
sbin/nginx -h  #查看所有nginx参数
```
}
```

# 参考
 - [CDH追加节点](https://cloud.tencent.com/developer/article/1078286)
