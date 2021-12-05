---
layout: posts
title: Docker for Mssql
date: 2021-11-28 22:31:43
tags: [Docker, MSSQL]
excerpt: windows docker安装MSSQL
categories: Mssql
index_img: https://gitee.com/xlzf/blog-image/raw/master/Home/20211201224941.jpeg
---

# Docker for SqlServer

![MSSQL](https://gitee.com/xlzf/blog-image/raw/master/Home/20211201224941.jpeg)

## 下载镜像

需要大概2G的空间

```shell
docker pull mcr.microsoft.com/mssql/server:2017-latest
```

*或者阿里云的*

``` shell
docker pull registry.cn-hangzhou.aliyuncs.com/newbe36524/server:2019-latest
```

## 创建映射目录

``` shell
D:\docker\SqlServer\data
```

## 运行

``` shell
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyMMSQL1433..."  -p 1433:1433 -v D:\docker\SqlServer\data:/var/opt/mssql/data --name mssql -d mcr.microsoft.com/mssql/server:2017-latest
```

注意：

参数中必须用双引号，否则:

``` shell
The SQL Server End-User License Agreement (EULA) must be accepted before SQL

Server can start. The license terms for this product can be downloaded from

...
```

![映射完毕](https://gitee.com/xlzf/blog-image/raw/master/Home/20211205003335.png)

然后，连接成功

![image-20211128232440826](https://gitee.com/xlzf/blog-image/raw/master/Home/20211128232443.png)

![image-20211128231956823](https://gitee.com/xlzf/blog-image/raw/master/Home/20211128231959.png)

![image-20211128232044403](https://gitee.com/xlzf/blog-image/raw/master/Home/20211128232047.png)

![image-20211128232508455](https://gitee.com/xlzf/blog-image/raw/master/Home/20211128232509.png)

