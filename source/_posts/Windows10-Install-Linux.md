---
title: Windows10 Install Linux
date: 2021-10-30 17:46:10
tags: [Linux, Ubuntu, Docker]
excerpt: 在Window10上通过Microsoft Store安装Ubuntu. 
categories: Linux 
index_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/Ubuntu.jpeg 
banner_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/4.jpg 
---

# window10 Install Linux 

## 准备工作

### 启动开发人员模式

![image-20210724160839938](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724160839938.png)

### 打开Linux子系统

![image-20210724161025846](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724161025846.png)

### Micrsoft Store 

![image-20210724160633196](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724160633196.png)

我自己使用的是 `Ubuntu 18.04 LTS`

![image-20210724161700603](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724161700603.png)

###  Ubuntu

![image-20210724163204041](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724163204041.png)

![image-20210724163230357](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724163230357.png)

设置好用户名和密码之后

![image-20210724163311586](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724163311586.png)

## 设置源

### 备份 source.list 

`sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak`

![image-20210724163701348](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724163701348.png)

本地路径：

`C:\Users\Harris\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState\rootfs\etc\apt`

阿里云源：

``` shell
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

清华源：

``` shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse

```

### 本文使用阿里云源

![image-20210724164013532](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724164013532.png)

## 初始化

注：update是更新软件列表，upgrade是更新软件。

### 更新软件列表

`sudo apt-get update`

这个命令，会访问源列表里的每个网址，并读取软件列表，然后保存在本地电脑。我们在新立得软件包管理器里看到的软件列表，都是通过update命令更新的。

### 更新软件

`sudo apt-get upgrade`

这个命令，会把本地已安装的软件，与刚下载的软件列表里对应软件进行对比，如果发现已安装的软件版本太低，就会提示你更新。

### 安装 xorg

`sudo apt-get install xorg`

### 安装xfce4

`sudo apt-get install xfce4`

### 安装xrdp

`sudo apt-get install xrdp`

### 配置xrdp

`sudo sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini`

### 向xsession中写入xfce4-session

`sudo echo xfce4-session >~/.xsession`

### 重启xrdp服务

`sudo service xrdp restart`

![image-20210724170258123](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724170258123.png)

上述命令执行完毕之后，通过本地IP+端口 ，例如: `192.168.31.64:3390` 去 `mstsc`  远程服务器，即可远程打开此`Ubuntu`。

## Docker

### 卸载之前的Docker

`sudo apt-get remove docker docker-engine docker.io`

如果想要卸载干净：

* 删除某软件,及其安装时自动安装的所有包

  ``` shell
  sudo apt-get autoremove docker docker-ce docker-engine  docker.io  containerd runc
  ```

* 删除docker其他没有没有卸载

  ``` shell
  dpkg -l | grep docker
  
  dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P # 删除无用的相关的配置文件
  ```

  

### 更新系统软件

`sudo apt-get update`

### 安装依赖包

```shell
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

### 添加官方密钥

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

显示OK,表示添加成功.

### 添加仓库

```shell
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

### 再次更新软件

`sudo apt-get update`

经实践，这一步不能够省略，我们需要再次把软件更新到最新，否则下一步有可能会报错。

### 安装docker

如果想指定安装某一版本，可使用 sudo apt-get install docker-ce=<VERSION>  命令，把<VERSION>替换为具体版本即可。

以下命令没有指定版本，默认就会安装最新版 

`sudo apt-get install docker-ce`

### 查看docker版本

`docker -v`

显示“Docker version 20.10.7, build f0df350”字样，表示安装成功。

### docker-compose安装

#### 下载docker-compose

`sudo curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose`

#### 授权

`sudo chmod +x /usr/local/bin/docker-compose`

![image-20210724175726176](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724175726176.png)

#### 查看版本信息

`docker-compose --version`

![image-20210724175854252](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724175854252.png)

### docker-machine安装

#### 安装virtualBox

`sudo apt install virtualbox`

#### 下载并安装docker-machine

```
curl -L https://github.com/docker/machine/releases/download/v0.13.0/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine &&
chmod +x /tmp/docker-machine &&
sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```

#### 查看版本信息

`docker-machine version`

![image-20210724180727779](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210724180727779.png)

#### 启动docker

- 启动服务：`sudo service docker start`


- 重启服务：`sudo service docker restart`



- 免sudo使用docker命令，添加一个docker group：`sudo groupadd docker`

- 将当前用户加入group组，然后退出并重新登录就可以了啦：`sudo gpasswd -a ${USER} docker`

- 启动docker:`sudo service docker start`

- 运行一个官方提供的hello-world镜像，并创建容器体验下：`sudo docker run hello-world`

### 加速

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://xxxxxx.mirror.aliyuncs.com"] 
  # 这个地方上各自的阿里云查看
  # https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 问题

#### 问题1

```
Error response from daemon: OCI runtime create failed: container_linux.go:380: starting container process caused: process_linux.go:722: waiting for init preliminary setup caused: EOF: unknown.
```

- 产生这个问题的原因

  目前我使用的Ubuntu的版本是18.04，我安装的是docker最新的版本，导致docker的版本和linux的内核版本不兼容

- 卸载当前的Docker

  `sudo apt-get autoremove docker-ce`

- 显示当前Ubuntu对应的可以使用的docker-ce可以使用的版本 

  `sudo apt-cache madison docker-ce`

- 在可以使用的版本中选择一个较为低的版本安装 

  `sudo apt-get install docker-ce=17.12.1~ce-0~ubuntu`
