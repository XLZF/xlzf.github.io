---
title: Docker Compose
date: 2022-01-19 16:46:15
tags: [Nginx, .net core, Docker]
excerpt: 通过Docker Compose 启动.net core web 应用程序并且使用Nginx反向代理
categories: Nginx
index_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220119155234027.png
---

# Docker Compose

![image-20220119155234027](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220119155234027.png)

## why docker compose

​		操作Docker的时候

​		如果只是操作单例服务的话，用docker命令操作:`build`、`run` 等等就能满足。

​		但是在操作一整套服务的时候，涉及到的容器操作就会巨多，运行多个微服务实例再加多个Consul做集群，然后再加上`Oracle`、`redis`、`Rabbitmq`等等，就会捉襟见肘。

​		所以需要`Docker compose`帮助做容器编排。

## 安装

Docker for windows 自带此功能

Linux 下：

从 Github 上下载它的二进制包来使用

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

网速慢,可以用daocloud下载

``` shell
sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

创建软链

``` shell
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

运行

```shell
root@vm02:~/webdemo# docker-compose --version
docker-compose version 1.25.1, build a82fef07
```

## 命令

``` shell
# 启动项目
docker-compose up

# 启动（后台运行）
docker-compose up -d

# 停止项目
docker-compose down 

# 项目日志
docker-compose logs

# 查看项目中的容器列表
docker-compose ps
```

## 应用

### 目标

通过`docker-compose` 启动 web 应用，同时使用`Nginx`反向代理。

![DockerCompose](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220119161032986.png)

### 开始

1. 将发布好的web应用文件放到服务器上。

![服务器web应用位置](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220119161132040.png)

2. 在服务器应用文件夹位置添加俩文件：`docker-compose.yml` 和 `proxy.conf`

   `docker-compose.yml` 是 `docker-compose`的默认配置文件。

   `proxy.conf` 是要给`nginx`挂载的配置文件，回头可以放别的地方。

![添加配置文件](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220119161212330.png)

3. `docker-compose.yml`

   `Docker-compose 配合文件`

   ``` yaml
   version: '2'
   services:
       webdemo:
           container_name: webdemo.compose
           build: .
           ports: 
            - "8081:80"
   
       reverse-proxy:
           container_name: nginxdemo
           image: nginx
           ports:
            - "8082:80"
           volumes:
            - ./proxy.conf:/etc/nginx/conf.d/default.conf
   ```

   释义：

   ``` shell
   version: '2'
   services:
       webdemo:
           container_name: webdemo.compose # 容器名称
           build: . # build 当前文件夹
           ports: 
            - "8081:80" # 端口映射
   
       reverse-proxy:
           container_name: nginxdemo # 容器名称
           image: nginx # 镜像名称
           ports:
            - "8082:80" # 端口映射
           volumes:
            - ./proxy.conf:/etc/nginx/conf.d/default.conf # 文件映射
   ```

4. `proxy.conf`

   `Nginx 配置`

   ``` c
   server {
     listen 80;
   
     location / {
       proxy_pass http://192.168.137.6:8081;
     }
   }
   ```

   释义：

   ``` c#
   server {
     listen 80; //监听端口
   
     location / {
       proxy_pass http://192.168.137.6:8081; //跳转url
     }
   }
   ```

### 运行

#### 启动

`docker-compose up -d`

``` shell
root@vm02:~/webdemo# docker-compose up -d
Creating network "webdemo_default" with the default driver
Creating webdemo.compose ... done
Creating nginxdemo       ... done
```

#### 启动日志

`docker-compose logs`

``` shell
root@vm02:~/webdemo# docker-compose logs
Attaching to nginxdemo, webdemo.compose
nginxdemo        | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
nginxdemo        | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
nginxdemo        | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
nginxdemo        | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
nginxdemo        | 10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
nginxdemo        | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
nginxdemo        | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
nginxdemo        | /docker-entrypoint.sh: Configuration complete; ready for start up
nginxdemo        | 2022/01/19 08:27:00 [notice] 1#1: using the "epoll" event method
nginxdemo        | 2022/01/19 08:27:00 [notice] 1#1: nginx/1.21.5
nginxdemo        | 2022/01/19 08:27:00 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
nginxdemo        | 2022/01/19 08:27:00 [notice] 1#1: OS: Linux 5.4.0-94-generic
nginxdemo        | 2022/01/19 08:27:00 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
nginxdemo        | 2022/01/19 08:27:00 [notice] 1#1: start worker processes
nginxdemo        | 2022/01/19 08:27:00 [notice] 1#1: start worker process 30
webdemo.compose  | warn: Microsoft.AspNetCore.DataProtection.Repositories.FileSystemXmlRepository[60]
webdemo.compose  |       Storing keys in a directory '/root/.aspnet/DataProtection-Keys' that may not be persisted outside of the container. Protected data will be unavailable when container is destroyed.
webdemo.compose  | warn: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[35]
webdemo.compose  |       No XML encryptor configured. Key {887cc5db-dcb2-4d26-886d-0f3aa1c787c9} may be persisted to storage in unencrypted form.
webdemo.compose  | info: Microsoft.Hosting.Lifetime[0]
webdemo.compose  |       Now listening on: http://[::]:80
webdemo.compose  | info: Microsoft.Hosting.Lifetime[0]
webdemo.compose  |       Application started. Press Ctrl+C to shut down.
webdemo.compose  | info: Microsoft.Hosting.Lifetime[0]
webdemo.compose  |       Hosting environment: Production
webdemo.compose  | info: Microsoft.Hosting.Lifetime[0]
webdemo.compose  |       Content root path: /app
```

![执行日志](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220119162936354.png)

#### 容器列表

`docker ps`

``` shell
root@vm02:~/webdemo# docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED              STATUS              PORTS                                   NAMES
0d4ea2075140   nginx             "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8082->80/tcp, :::8082->80/tcp   nginxdemo
686adcb2655a   webdemo_webdemo   "dotnet WebDemo.dll"     About a minute ago   Up About a minute   0.0.0.0:8081->80/tcp, :::8081->80/tcp   webdemo.compose
```

![容器列表](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220119163032471.png)

#### 访问

![运行结果](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220119163327957.png)

## 最后

至此，已经完成了通过`Docker-compose`动态编排容器，实现`Nginx`反向代理`.Net core web 应用程序`

随着学习的深入，后续也会渐渐丰满本篇内容。