---
title: Docker for RabbitMq
date: 2022-03-19 20:00:52
tags: [Docke]
excerpt: Docker 安装 RabbitMQ
categories: RabbitMq
index_img: https://gitee.com/xlzf/blog-image/raw/master/Home/a5d19d60e8798247f2a9b360ef003e20.jpeg
---

# Docker for RabbitMQ

![](https://gitee.com/xlzf/blog-image/raw/master/Home/a5d19d60e8798247f2a9b360ef003e20.jpeg)

## run

``` shell
docker run -dit --name Myrabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin -p 15672:15672 -p 5672:5672 rabbitmq:management
```

```text
-d                          #后台运行
-- homename fuyi-rabbit     #主机名
RABBITMQ_DEFAULT_USER=admin #可视化界面登录用户名
RABBITMQ_DEFAULT_PASS=admin #可视化界面登录密码
-p 15672:15672              #端口映射
c20                         #镜像ID
```

  

测试 gitee Hexo
