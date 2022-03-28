---
layout: posts
title: Docker for Oracle_xe_11g
date: 2021-11-28 22:16:52
tags: [Docker, Oracle]
excerpt: windows docker 安装 Oracle XE 11g
categories: Oracle
index_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/Oracle.jpeg
---

# Docker下安装oracle-xe-11g

## 下载镜像

``` shell
docker pull docker.io/sath89/oracle-xe-11g
```

## 创建容器

使用docker run命令创建一个容器，-d表示在后台运行容器，并打印容器ID，-p表示映射一个宿主机的端口号，--name表示重命名容器，如果不指定容器名，docker会默认为容器起一个别名，-v表示创建一个数据卷，将宿主机的某个目录和容器挂载到一起

```shell
docker run -d -p 1521:1521 --name oracle -v /u01/app/oracle/:/u01/app/oracle/ sath89/oracle-xe-11g
# 用下面这个吧 映射到本地文件夹
docker run -d -p 1521:1521 --name oracle -v D:\docker\Oracle_XE_11g\data:/u01/app/oracle/ sath89/oracle-xe-11g
```

## 查看容器是否创建成功

```shell
docker ps
```

## 出现以下内容说明容器创建成功

```shell
[root@bogon ~]# docker ps
CONTAINER ID        IMAGE                  COMMAND             CREATED             STATUS              PORTS                                            NAMES
8e0d664a262a        sath89/oracle-xe-11g   "/entrypoint.sh "   7 weeks ago         Up 27 hours         0.0.0.0:1521->1521/tcp, 0.0.0.0:8080->8080/tcp   oracle
[root@bogon ~]# 
```

## 进入容器内部

```shell
docker exec -it oracle /bin/bash
```

## 在容器内部使用sqlplus连接oracle

```
root@8e0d664a262a:/# sqlplus
SQL*Plus: Release 11.2.0.2.0 Production on Sat Jul 20 09:30:42 2019

Copyright (c) 1982, 2011, Oracle.  All rights reserved.

Enter user-name: system
Enter password: 

Connected to:
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production

SQL> 
```

oracle默认的用户名密码为：system oracle

## Docker 命令行

``` shell
#进入容器命令行

docker exec -it oracle /bin/bash

#容器默认时区为UTC,换成上海时间：

cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

## 常用SQL

```sql
--创建临时表空间示例
CREATE TEMPORARY TABLESPACE XLZF_TableSpace
 
TEMPFILE '/u01/app/oracle/oradata/XE/XLZF_temp.dbf'
 
SIZE 32M
 
AUTOEXTEND ON
 
NEXT 32M MAXSIZE UNLIMITED
 
EXTENT MANAGEMENT LOCAL;

-- 创建表攻坚示例
CREATE TABLESPACE XLZF_TableSpaces
 
LOGGING
 
DATAFILE '/u01/app/oracle/oradata/XE/XLZF.dbf'
 
SIZE 32M
 
AUTOEXTEND ON
 
NEXT 32M MAXSIZE UNLIMITED
 
EXTENT MANAGEMENT LOCAL;

--创建用户示例
CREATE USER XLZF IDENTIFIED BY XLZF
 
ACCOUNT UNLOCK
 
DEFAULT TABLESPACE XLZF_TableSpaces
 
TEMPORARY TABLESPACE XLZF_TableSpace;

--用户授权示例

GRANT CONNECT,RESOURCE TO XLZF;

```

参考学习地址：[docker运行oracle xe 11g | 编程小战 (joycode.com.cn)](http://www.joycode.com.cn/archives/367)

# Docker 下安装Oracle-11g

## 拉取镜像

``` shell
docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```

## 启动测试用例

```shell
docker run -d --name test --restart unless-stopped -p 1521:1521 registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```

## 创建路径

``` shell
D:\docker\Oracle11g\data
```

## 映射路径

``` shell
docker cp test:/home/oracle/app/oracle/oradata/helowin D:\docker\Oracle11g\data
```

## 重新启动

``` shell
docker run -d --name oracle11g --restart unless-stopped -v D:\docker\Oracle11g\data\helowin:/home/oracle/app/oracle/oradata/helowin -p 1521:1521 registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```

不知道是什么原因，本地每次拉取完阿里云的Oracle11g镜像，都run不了，稍有遗憾，但是XE可以。
