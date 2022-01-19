---
title: net core web + Docker + Nginx
date: 2022-01-19 14:23:15
tags: [Nginx, .net core, Docker]
excerpt: 在Docker中通过Nginx反向代理访问.Net Core web 应用
categories: Nginx
index_img: https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119165020105.png
---

# .NetCore Docker Nginx

## Test Program

### 新建 .net core web应用

![新建应用](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119135939441.png)

### 添加Docker 支持-linux

![docker支持](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119140022112.png)

### 发布

![发布设置](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119140104644.png)

### 修改发布后的DockerFile

![发布后的文件](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119140144967.png)

![修改DockerFile](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119140208877.png)

```shell
#See https://aka.ms/containerfastmode to understand how Visual Studio uses 
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

WORKDIR /app
COPY .  /app
ENTRYPOINT ["dotnet", "WebDemo.dll"]
```

### 上传到服务器

![xftp](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119140355752.png)

## Docker image

### build test program

![testProgramBuild](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119140542103.png)

### pull nginx

``` shell
docker pull nginx
```

## Docker Run

### run test program image

``` shell
docker run -d -p 5000:80 --name webdemo.nginx webdemo:v1.0
```

### 添加Nginx配置

`myNginx.conf`,`192.168.137.6`为宿主IP

``` shell
vim /root/nginx/my_nginx.conf
# 内容
server {
  listen 80;

  location / {
    proxy_pass http://192.168.137.6:5000;
  }
}
```

### run nignx image

挂载到/root/nginx/my_nginx.conf

```shell
docker run -d -p 8080:80 -v /root/nginx/my_nginx.conf:/etc/nginx/conf.d/default.conf nginx
```

## Last

本篇主要是将.Net core web应用放到Linux服务器中，使用Nginx反向代理，进行访问。

![docker容器列表](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119142358772.png)

![web应用服务](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119142220882.png)

![ngixn代理端口](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220119142258548.png)



