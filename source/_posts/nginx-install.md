---
title: nginx安装
tags: 
    - nginx
categories: 
    - nginx
---

# nginx安装

## 下载

1.  <http://nginx.org/en/download.html> nginx官网下载
2.  yum install nginx ‐y 命令下载

> yum 是 CentOS中的Shell前端软件包管理器

## 环境说明 不懂

1.  确定Linux的内核版本要在2.6以上，只有2.6之后才支持epool   ，在此之前使用select或pool多路复用的IO模型，无法解决高并发压力的问题。通过命令uname -a 即可查看
2.  需要下载额外的包(zlib库、OpenSSL开发库、PCRE库、GCC编译器)

> yum -y install make zlib zlib-devel gcc-c++ libtool openssl openssl-devel pcre pcre-devel

## 解压安装

将下载好的安装包进行解压

> tar -zxvf nginx-1.16.0.tar.gz

解压完毕进入nginx文件夹 依次执行命令安装

> ./configure

> make

> make install

安装完毕之后进入安装目录

> cd  /usr/local/nginx  (可能不是这个)

启动nginx

```
#默认方式启动：

./sbin/nginx

#指定配置文件启动

./sbing/nginx -c /tmp/nginx.conf

#指定nginx程序目录启动

./sbin/nginx -p /usr/local/nginx/

```

停止nginx

    #快速停止

    ./sbin/nginx -s stop

    #优雅停止

    ./sbin/nginx -s quit

其它命令：

```
# 热装载配置文件 ，不用停止可以刷新配置（一定要熟练，这是用的最多的命令）

./sbin/nginx -s reload

# 重新打开日志文件（下面单说）

./sbin/nginx -s reopen

# 检测当前使用的是哪个配置文件，配置是否正确（可以在配置文件加点乱码测试一下）（这个命令也经常使用）

./sbin/nginx -t

```

