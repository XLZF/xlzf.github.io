---
title: Docker+GitLab+Jenkins
date: 2022-01-20 22:47:51
tags: [jenkins, Docker, GitLab, .net core]
excerpt: Docker+GitLab+Jenkins 自动部署 DotNet Core Application
categories: MicroService
index_img: https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220120225149050.png
---

![Jenkins](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220120225149050.png)

## 前言

大概意图：`Docker`+`GitLab`+`Jenkins` 自动部署 `Dot Net Core Web Application`

![大概意图](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220120211456716.png)

## 思路

![主要容器](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220120212431735.png)

## GitLab

### 镜像下载

``` shell
docker pull gitlab/gitlab-ce
```

或者 导入本地镜像

``` shell
docker load < /root/dockerimage/xxxx.tar
```

### 运行

``` shell
docker run -d  -p 443:443 -p 8888:80 -p 222:22 --name gitlab --restart always \
-v /root/gitlab/config:/etc/gitlab \
-v /root/gitlab/logs:/var/log/gitlab \
-v /root/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce
```

### 添加秘钥

``` shell
ssh-keygen -t rsa -C 'xxx@xxx.com'
```





## Jenkins

### 镜像下载

``` shell
docker pull jenkins/jenkins
```

### 运行镜像

``` shell
docker run -u root -d -p 8080:8080 -p 50000:50000 -v /root/jenkins_data/DockerData:/var/jenkins_home -v /root/jenkins_data/docker.sock:/var/run/docker.sock jenkins/jenkins --restart always
```

### 问题1

>  Please wait while Jenkins is getting ready to work ...

``` shell
# 打开
hudson.model.UpdateCenter.xml   #这个就是挂载的root/jenkins_data/Dockerdata里
# 把
http://updates.jenkins-io/update-center.json
# 改成 清华大学镜像地址
http://mirror.xmission.com/jenkins/updates/update-center.json
```

### 问题2

> 容器内执行`apt-get update` 总超时，需要添加源

``` shell
# 将容器内的配置文件导出来
docker cp elegant_neumann:"/etc/apt/sources.list" /root/jenkins_config/
# 将修改好的配置文件覆盖到容器中
docker cp "/root/jenkins_config/sources.list" elegant_neumann:"/etc/apt/sources.list"
```

### 问题3

> 更新源后会报错：`NO_PUBKEY 40976EAF437D05B5 NO_PUBKEY 3B4FE6ACC0B21F32`

``` shell
apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 40976EAF437D05B5 3B4FE6ACC0B21F32
```

然后添加ping `apt install iputils-ping`

### 问题4

> 因为要在`Jenkins`中运行 `DotNet 5.0 Web`，所以需要在`Jenkins`容器内有 `dotnet 5.0 SDK`,这个问题会体现在构建的时候。
>
> 这涉及到通过`Dockerfile` 创建镜像，这里需要创建一个携带`dotnet5.0 SDK`的`jenkins`镜像

参考地址：[.NET 5 + Docker Jenkins，做自动化部署，全Docker环境](https://blog.csdn.net/feng005211/article/details/114818504)

``` shell
# 封装Jenkins镜像（带有dotnet环境的） sdk=5.1
FROM jenkins/jenkins:lts
USER root
WORKDIR /dotnet
RUN wget -O dotnet.tar.gz https://download.visualstudio.microsoft.com/download/pr/820db713-c9a5-466e-b72a-16f2f5ed00e2/628aa2a75f6aa270e77f4a83b3742fb8/dotnet-sdk-5.0.100-linux-x64.tar.gz
RUN tar zxf dotnet.tar.gz -C ./
RUN rm -rf dotnet.tar.gz
ENV PATH="${PATH}:/dotnet:/var/jenkins_home/.dotnet/tools"
ENV DOTNET_ROOT="/dotnet"
RUN apt update -y
RUN apt install icu-devtools vim zip unzip -y
RUN usermod -a -G root jenkins
USER jenkins
```

```
命令解释
1.这个Docker镜像基于jenkins
2.设置当前用户为root，因为后面安装需要使用root
3.设置当前工作目录为dotnet
4.下载dotnet SDK包，保存为dotnet.tar.gz。这里要注意下载正确版本的SDK，可前往微软官方网站获取下载链接：https://dotnet.microsoft.com/download
5.解压dotnet SDK到当前目录，即/dotnet目录
6.删除dotnet SDK包
7.把dotnet目录和dotnet tools目录添加到环境变量PATH，这样就可以使用dotnet命令了
8.设置DOTNET_ROOT变量
9.更新源
10.安装一些必需的，常用的工具包，其中icu-devtools是运行dotnet需要的
11.修改jenkins用户到root附加组
12.设置当前用户为jenkins
```

```
构建镜像 name=jenkins:dotnet
docker build -t jenkins:dotnet .
很简单的将包含dotnet环境的jenkins安装好了
```
