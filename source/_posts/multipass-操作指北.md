---
layout: posts
title: multipass 操作指北
date: 2022-01-18 09:53:26
tags: [multipass, ubuntu]
excerpt: windows 10 multipass配合Hype-v管理器使用
categories: Ubuntu
index_img: https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220113152838526.png 
---

# Multipass 操作指北

![multipass](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220113152838526.png )

## 前言

由于学习需要操作`Docker`，有之前使用的`VM14PRO`+`CentOS7`+`Ubuntu`,还有就是`docker desktop for windows`,现在发现`multipass` ，忍不住想试试。

本篇其实是想以虚拟机子系统安装Docker，Docker中安装Consul，模拟Consul集群，客户端进行访问。

![image-20220118135857819](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118135857819.png)

## 下载安装

* [下载]([Multipass orchestrates virtual Ubuntu instances](https://multipass.run/))
* 安装完成之后，右下角托盘中能看到图标，右键图标即可运行shell

## 基本操作

### 查看版本

`win+R` 运行 `cmd`  , 查看版本 `multipass version`

### 查看虚拟机列表

``` shell
multipass ls
```

![虚拟机列表](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118101606479.png)

### 新建虚拟机

创建虚拟机
语法：multipass launch -n 虚拟机名称
-n, --name: 名称
-c, --cpus: cpu核心数, 默认: 1
-m, --mem: 内存大小, 默认: 1G
-d, --disk: 硬盘大小, 默认: 5G

``` shell
multipass launch -n ubuntu-lts -c 4 -m 4G -d 40G
```

![新建虚拟机](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118101648294.png)

### 进入虚拟机

``` shell
multipass shell 虚拟机名称
```

![进入虚拟机](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118101802147.png)

### 不进入虚拟机直接执行命令

``` shell
multipass exec ubuntu-lts -- ls #语法：multipass exec 虚拟机名称 --命令
```

![image-20220118102055691](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118102055691.png)

### 查看虚拟机信息

``` shell
multipass info 虚拟机名称
```

![image-20220118102212512](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118102212512.png)

### 重启虚拟机

``` shell
multipass restart 虚拟机名称
```

### 删除虚拟机

``` shell
# 普通删除（可恢复）
multipass delete 虚拟机名称
# 彻底删除
multipass delete --purge 虚拟机名称 
```

### 恢复删除虚拟机

``` shell
multipass recover 虚拟机名称 
```

### 启动虚拟机

``` shell
multipass start 虚拟机名称 
```

### 暂停虚拟机

```shell
multipass stop 虚拟机名称 
```

### 挂载宿主机目录

``` shell
multipass mount 宿主机目录 虚拟机名称:虚拟机目录
```

### 卸载挂载目录

```shell
multipass unmount 虚拟机名称:虚拟机目录
```

## Ubuntu

现在multipass 默认安装的是 Ubuntu 20.04.3 LTS

### 设置源

``` shell
sudo vim /etc/apt/sources.list
```

``` shell
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
```

### 修改 root pwd

``` shell
sudo passwd root
```

### 更新 apt 包索引

``` shell
sudo apt-get update
```

### ifconfig

``` shell
sudo apt install net-tools
```

### 安装Docker

#### 使用 Docker 仓库进行安装

1. 安装 apt 依赖包

```shell
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

2. 添加 Docker 的官方 GPG 密钥：

```shell
$ curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

3. 验证您现在是否拥有带有指纹的密钥

```shell
$ sudo apt-key fingerprint 0EBFCD88
   
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

4. 使用以下指令设置稳定版仓库

``` shell
$ sudo add-apt-repository \
   "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/ \
  $(lsb_release -cs) \
  stable"
```

#### 安装 Docker Engine-Community

1. 更新 apt 包索引。

``` shell
sudo apt-get update
```

2. 安装最新版本的 Docker Engine-Community 和 containerd

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

3. 测试Docker

``` shell
sudo docker run hello-world
```

#### 开机自启容器

``` shell
systemctl enable docker.service
```

#### 获取Consul

``` shell
docker pull consull
```

#### 运行Consul

``` shell
docker run --publish 8600:8600 --publish 8500:8500 --publish 8300:8300 --publish 8301:8301 --publish 8302:8302 --name consul-01 --restart always \
--volume /root/docker/consulone/data:/consul/data \
--volume /root/docker/consulone/config:/consul/config consul:latest agent --server --bootstrap-expect=1 --ui --bind=0.0.0.0 --client=0.0.0.0 

docker run --publish 8501:8500 --name consul-02 --restart always \
--volume /root/docker/consultwo/data:/consul/data \
--volume /root/docker/consultwo/config:/consul/config consul:latest agent --server --ui --bind=0.0.0.0 --client=0.0.0.0 --join 172.17.0.2

docker run --publish 8502:8500 --name consul-03 --restart always \
--volume /root/docker/consulthree/data:/consul/data \
--volume /root/docker/consulthree/config:/consul/config consul:latest agent --server --ui --bind=0.0.0.0 --client=0.0.0.0 --join 172.17.0.2

# http://*:8500  172.17.0.2

# http://*:8501  172.17.0.3

# http://*:8502  172.17.0.4
```

## 开启SSH登录

使用`xshell and xftp`

### 安装

```shell
sudo apt-get install openssh-server
```
### 验证开启SSH

``` shell
ps -e |grep ssh 
```

``` shell
dpkg --get-selections | grep ssh
```

![image-20220118104032656](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118104032656.png)

如果看到sshd那说明ssh-server已经启动了。

如果没有则可以这样启动 `sudo /etc/init.d/ssh start`

### 配置

ssh-server配置文件位于`/etc/ssh/sshd_config`，在这里可以定义SSH的服务端口，默认端口是22，你可以自己定义成其他端口号，如222。

``` shell
Port 22
LoginGraceTime 2m
PermitRootLogin without-password
PermitRootLogin yes
StrictModes yes
RSAAuthentication yes
PubkeyAuthentication yes
```

别的看着弄，上面这几个配置弄好了就成。

![image-20220118104744600](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118104744600.png)

### RSA秘钥

打开xshell，工具-用户秘钥管理者-生成

![下一步](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118105251888.png)

![下一步](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118105307456.png)

![完成](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118105346104.png)

![这么点](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118105443765.png)

![点这里](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118105607833.png)

![喏，这就是那个公钥](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118105648372.png)

这个秘钥需要复制到Ubuntu 里。

先打开 `cd ~/.ssh`

![定位到文件](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118105927900.png)

打开之后就是下面这些，把下面的内容替换成上面生成的公钥即可。

![image-20220118105956280](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118105956280.png)

![这么粘](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118110131201.png)

![粘贴公钥](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118110222578.png)

### 连接xshell

先获取IP`ifconfig`,没有的话安装一下`sudo apt install net-tools`

![获取当前IP](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118110413466.png)

配置xshell连接属性

![连接xshell](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118110734124.png)

![设置用户密码](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118110800257.png)

点击连接

![接收并保存](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118110834655.png)

点击接收并保存，选择Public Key，输入刚才设置的密码，点击确定

![选择秘钥填充密码](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118110924974.png)

连接成功

![连接成功](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220118111009999.png)

## 设置固定IP

每次重启IP都变了，xshell还得重新配置IP，设置固定IP搞定

### 首先

想要固定IP，需要在Hype-v 管理器中添加

![image-20220116145911179](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116145911179.png)



### 然后新建虚拟机交换机

![image-20220116150220744](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150220744.png)

### 设置交换机IP

![image-20220116150314941](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150314941.png)

![image-20220116150340431](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150340431.png)

![image-20220116150402104](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150402104.png)

### 分享Internet

设置完IP之后，

需要Internet访问的话，需要将现在宿主用的网络分享给vethernet(新建虚拟机)

![image-20220116150646604](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150646604.png)

也就是说在 宿主再用的WLAN里点击属性，点击共享选择 vEthernet（新建虚拟交换机），就是刚才咱新建的内部交换机。

### 修改配置文件

至此，宿主这块设置好了。接下来需要设置子系统 Ubuntu。

先执行

```shell
vim /etc/netplan/50-cloud-init.yaml
```

insert 模式  i

![image-20220116151058994](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116151058994.png)

``` shell
network:
    ethernets:
        eth0:
            dhcp4: no
            addresses: [192.168.137.6/24]
            optional: true
            gateway4: 192.168.137.1
            nameservers:
                addresses: [8.8.8.8,114.114.114.114]
    version: 2
```

修改完配置之后，重启网卡。

```shell
sudo netplan apply
```

如果通过multipass 的shell 去执行 `sudo netplan appyly`的话，执行完不报错也没啥反应

也可以选择在 hype-v 中连接 ，在shell中去输入

### 最后

可以输入 `ifconfig`

如果报错 需要升级 `sudo apt install net-tools `

![image-20220116151509554](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116151509554.png)

不要在意ip不对，我一共启动了三个虚拟机，分别是 5、6、6为后缀的IP

### 现在

固定IP算是设置完毕了，超简单.

## 安装 FTP

``` shell
sudo apt-get install vsftpd
# 设置开机启动并启动ftp服务
systemctl enable vsftpd
systemctl start vsftpd
#查看其运行状态
systemctl  status vsftpd
#重启服务
systemctl  restart vsftpd
```

``` shell
#修改配置
sudo /etc/vsftpd.conf
```

``` shell
#存在的修改成酱紫：
listen=NO # 阻止 vsftpd 在独立模式下运行
listen_ipv6=YES # vsftpd 将监听 ipv6 而不是 IPv4，你可以根据你的网络情况设置
anonymous_enable=NO # 关闭匿名登录
local_enable=YES # 允许本地用户登录
write_enable=YES # 启用可以修改文件的 FTP 命令
local_umask=022 # 本地用户创建文件的 umask 值
dirmessage_enable=YES # 当用户第一次进入新目录时显示提示消息
xferlog_enable=YES # 一个存有详细的上传和下载信息的日志文件
connect_from_port_20=YES # 在服务器上针对 PORT 类型的连接使用端口 20（FTP 数据）
xferlog_std_format=YES # 保持标准日志文件格式
pam_service_name=vsftpd # vsftpd 将使用的 PAM 验证设备的名字
```

