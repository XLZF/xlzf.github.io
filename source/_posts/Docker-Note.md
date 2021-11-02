---
title: Docker Note
date: 2021-10-31 19:13:24
tags: [Docker, .net core]
excerpt: 在window10中部署Docker环境运行dotnetcore web app为主线记录过程。
categories: C#
index_img: https://gitee.com/xlzf/blog-image/raw/master/docker.jpeg
---

# Docker 学习笔记

> 本文以在 window  10 中部署 Docker 环境 运行 dotnet core web app 为主线 记录过程

## 环境部署

### 下载Docker 

[下载Docker]: https://hub.docker.com/editions/community/docker-ce-desktop-windows	"傻瓜式安装即可"

![image-20210530131053540](https://gitee.com/xlzf/blog-image/raw/master/X6LQMJNwKADEv4f.png)

### window 开启虚拟化

![image-20210530162435983](https://gitee.com/xlzf/blog-image/raw/master/image-20210530162435983.png)

### 新建.net core web app

####  新建程序

<img src="https://gitee.com/xlzf/blog-image/raw/master/image-20210530151814645.png" alt="image-20210530151814645" style="zoom:80%;" />

#### 设置参数

<img src="https://gitee.com/xlzf/blog-image/raw/master/image-20210530151929348.png" alt="image-20210530151929348" style="zoom:80%;" />

#### 运行程序

> 默认运行是Docker ,这里先选择 helloworldapp 运行

<img src="https://gitee.com/xlzf/blog-image/raw/master/image-20210530152509982.png" alt="image-20210530152509982" style="zoom:80%;" />

1. 运行项目没有问题

   <img src="https://gitee.com/xlzf/blog-image/raw/master/image-20210530152231219.png" alt="image-20210530152231219" style="zoom:80%;" />



4. 需要注意此文件

   <img src="https://gitee.com/xlzf/blog-image/raw/master/image-20210530152604550.png" alt="image-20210530152604550" style="zoom:80%;" />

#### Dockerfile

> 把Dockerfile 从当前文件夹放到上一级中。

![image-20210530153921848](https://gitee.com/xlzf/blog-image/raw/master/image-20210530153921848.png)

![image-20210530153951697](https://gitee.com/xlzf/blog-image/raw/master/image-20210530153951697.png)

至此，环境需要运行的程序准备完毕。

#### PowerShell 

下一步，打开 powershell 程序。

![image-20210530154105810](https://gitee.com/xlzf/blog-image/raw/master/image-20210530154105810.png)

cd 到指定工程目录 `E:\Code\testDocker\helloworldapp`

![image-20210530154622261](https://gitee.com/xlzf/blog-image/raw/master/image-20210530154622261.png)

#### 命令

`docker images`: 查看 当前 image 

`docker ps` : 查看当前容器

`docker build -t myapp:v1.0 . ` : 创建镜像 `myapp:v1.0 `, 最后` .` 代表在当前文件夹中开始执行。`v1.0` 是Tag ,算是版本号把。

依照上图操作流程，`build` 操作完成之后，执行 `docker images`,就会看到 一个列表，中间记录了名称、版本号、image id、创建时间和大小。执行`docker ps` 没有内容是因为还没有运行image。

下一步执行

`docker run --name=myapp -p 7778:80 -d myapp:v1.0`

![image-20210530161559489](https://gitee.com/xlzf/blog-image/raw/master/image-20210530161559489.png)

出现一串字符串的时候，说明容器启动成功了，这个时候再执行`docker ps`,就会出现如下列表：

![image-20210530161653125](https://gitee.com/xlzf/blog-image/raw/master/image-20210530161653125.png)

此时如果访问本地端口，如下图

![image-20210530161829323](https://gitee.com/xlzf/blog-image/raw/master/image-20210530161829323.png)

#### Docker for Desktop

> 在软件：window desktop docker 中也会体现出image 和 docker(容器)

image 

![image-20210530161957344](https://gitee.com/xlzf/blog-image/raw/master/image-20210530161957344.png)

docker 

![image-20210530162042171](https://gitee.com/xlzf/blog-image/raw/master/image-20210530162042171.png)

与此同时，也可以通过 软件 进行 docker 程序的访问

![image-20210530162219487](https://gitee.com/xlzf/blog-image/raw/master/image-20210530162219487.png)

## 常用命令

### 创建镜像

`docker build -t myapp:v1.0 . //myapp 这个是自定义的但是不能有大写`

### 运行容器

`docker run --name=myapp -p 7778:80 -d myapp:v1.0` 

`docker run --name=[容器名称] -p [宿主机端口]:[Docer本地端口] -d [镜像名称]:[标签]`

### 删除指定镜像

`docker vmi id //image id`

### 查看镜像

`docker images //查看image list`

### 查看容器

`docker ps` | `docker ps -a`  *ps：-a (全部)*

### 保存镜像文件

`docker save -o 要保存的文件名  要保存的镜像` 

这个命令保存没有问题，但是还原的时候会导致镜像和标签为空

推荐使用：

`docker save -o myapp.tar myapp:v1.0` 这种：`docker save -o [镜像名称].tar [镜像名称]:[标签]`

### 导入镜像文件

`docker load --input 文件` 或者 `docker load < 文件名`

### 停止容器

`docker stop [容器ID]` *实在不想等待就直接 `docker kill [容器ID]`*

### 停止所有的容器

`docker stop $(docker ps -q)`

### 重启容器

`docker restart [容器ID]`

### 强制移除容器

`docker rm -f mysql1`

### 重命名镜像

`docker tag [镜像id] [新镜像名称]:[新镜像标签]`

### 查看日志

`docker logs`

## Docker 数据持久化

> 如果不做数据持久化的话，容器中的数据在关闭的时候就会丢失，反之，就算容器删除对数据没有影响。

### 数据持久化策略之`volumes`

- 多个容器这间共享数据
- 宿主机不保证存在固定的目录结构
- 持久化数据到远程主机或者云存储而非本地
- 需要备份、迁移、合并数据时。停止container，将volume整体复制，用于备份、迁移、合并等。

### 数据持久化策略之`bind mount`

本质上是宿主机、container之间共享宿主机文件系统。这种持久化方法更导致container与宿主机的耦合过于紧密，所以不推荐使用。

container共享宿主机配置文件。

比如docker会将宿主机文件/etc/resov.conf文件bind mount到容器上，两者会使用相同的DNS服务器。

开发环境中宿主机与container之间共享源代码、构建构件等。比如将整个build过程container化，将宿主机上的源代码文件夹bind mount到build container中。修改代码后，运行build container的build命令，build container则将build结构写入另一个bind mount的文件夹中。

一些监控类container，通过读取宿主机固定文件中的数据实现监控等。

### Mongo 

``` shell
# 拉取镜像
docker pull mongo:4.0.22

# 启动容器，挂载本地目录
docker run -itd --name mongo -p 27017:27017 -v $PWD/mongodb:/data/db mongo:4.0.22
    
# 2021-06-04
    
# 创建目录
mkdir /data/mongodb
    
# 启动容器
//docker run --name mongodb -v /data/mongodb:/data/db -p 27017:27017 -d 07630
-- 20210605 D 盘挂载成功 https://www.cnblogs.com/codelove/p/10312692.html
docker run -p 27017:27017 --name myMongodb -v d:/data/mongodb:/data/db -d mongo
    
# 进入MongoDB
docker exec -it 95ed5df9fc2c mongo admin //95ed5 是容器ID
```

### MongoDB Shell 

``` shell
1. 数据库内容

   //查看所有数据库
   show dbs

   //删除数据库
   db.dropDatebase()

2. 集合内容

   //创建集合
   db.createCollection()

   //查看所有集合\表
   show collections
   show tables

   //选定某一集合
   use db_name

   //查看集合的信息
   db.stats()

   //删除一个集合，但是需要先指定一个数据库，即先执行 use db_name
   db.dropDatabase()

   //修改集合的名称
   db.collection_name.renameCollection('new_name')

3. 文档内容

   //插入数据
   db.collection_name.insert(document)
   db.collection_name.save(document)

   //查询数据多条数据
   db.collection_name.find()
   
   //查询 当前数据中 test 集合里，有没有name = zhangsan 的对象
   db.test.find({name:"zhangsan"});
```

## Docker Compose

> Docker-Compose项目是Docker官方的开源项目，负责实现对Docker容器集群的快速编排

> 如果安装的是 window desktop Docker 自带 Docker Compose

### 查看版本

`docker-compose --version`

### Docker to MongoDB

1. 安装

   `docker pull mongo`

2. 运行

   `docker run -itd --name mongo -p 27017:27017 mongo --auth`

3. 使用以下命令添加用户和设置密码，并且尝试连接

   ``` c
   docker exec -it mongo mongo admin
   //创建一个名为 admin，密码为 123456 的用户。
   > db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},"readWriteAnyDatabase"]});
   //尝试使用上面创建的用户信息进行连接。
   > db.auth('admin', '123456')
   ```

## Docker Swarm 集群



## 加速

> 镜像加速以下 或者考虑 阿里云镜像加速 
>
> [阿里云Docker 加速]: https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

``` json
{
  "registry-mirrors": [
    "https://1rlt72n0.mirror.aliyuncs.com",
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://reg-mirror.qiniu.com",
    "https://dockerhub.azk8s.cn",
    "https://mirror.ccs.tencentyun.com"
  ],
  "insecure-registries": [],
  "debug": true,
  "experimental": false,
  "features": {
    "buildkit": true
  },
  "builder": {
    "gc": {
      "enabled": true,
      "defaultKeepStorage": "20GB"
    }
  }
}
```

## 常用报错

>  安装Docker Desktop报错 `WSL `2 installation is incomplete.

安装：https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

> 电脑启动后，Docker 没有启动，执行如下命令

`cd "C:\Program Files\Docker\Docker" DockerCli.exe -SwitchDaemon`
