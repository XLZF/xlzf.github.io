---
layout: posts
title: Docker for MongoDB
date: 2021-12-01 22:34:32
tags: [Docker, MongoDB]
excerpt: windows docker安装MongoDB
categories: MongoDB
index_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/20211201225103.jpeg
---

# Docker for MongoDB

![](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/20211201225103.jpeg)

## 安装镜像

`docker pull mongo`

## 持久化

`docker run --name mongo -v D:\docker\MongoDB\data:/data/db -d -p 27017:27017 mongo`

`D:\docker\MongoDB\data`为本地目录，作为数据持久化文件夹。

## 配置账号密码

```shell
docker exec -it [镜像ID]/bin/sh
# mongo admin
```

## 创建Mongo账号

```shell
//选择admin数据库
> use admin 
//创建账号
> db.createUser({user: 'admin', pwd: 'admin123456', roles: [{role: "userAdminAnyDatabase", db: "admin" }]}); 
//测试账号
>  db.auth('admin', 'admin123456') 
```

## 登录测试

![MongoDB](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/20211201223653.png)
