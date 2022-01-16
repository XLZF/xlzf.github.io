---
layout: posts
title: multipass ubuntu 固定IP
date: 2022-01-16 11:56:08
tags: [multipass, ubuntu]
excerpt: multipass 安装 ubuntu 20.04 TLS 固定IP
categories: Ubuntu
index_img: https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220113152838526.png
---

# Multipass Ubuntu 20.04 TLS 固定IP

![image-20220113152838526](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20220113152838526.png)

## 首先

为啥要固定IP

multipass launch -n vm04 之后，一般先通过 multipass 自带的 Open Shell 去执行命令

比如 设置 root 密码

``` shell
# 设置 root 密码
sudo passwd root
root
root
```

如果是宿主是 window 系统的话，可以启动 hype-v 管理器 去连接查看



先说一下固定IP的事

想要固定IP，需要在Hype-v 管理器中添加

![image-20220116145911179](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116145911179.png)



## 然后新建虚拟机交换机

![image-20220116150220744](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150220744.png)

## 设置交换机IP

![image-20220116150314941](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150314941.png)

![image-20220116150340431](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150340431.png)

![image-20220116150402104](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150402104.png)

## 分享Internet

设置完IP之后，

需要Internet访问的话，需要将现在宿主用的网络分享给vethernet(新建虚拟机)

![image-20220116150646604](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116150646604.png)

也就是说在 宿主再用的WLAN里点击属性，点击共享选择 vEthernet（新建虚拟交换机），就是刚才咱新建的内部交换机。

## 修改配置文件

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
            addresses: [192.168.137.5/24]
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

## 最后

可以输入 `ifconfig`

如果报错 需要升级 `sudo apt install net-tools `

![image-20220116151509554](https://gitee.com/xlzf/blog-image/raw/master/Home/image-20220116151509554.png)

不要在意ip不对，我一共启动了三个虚拟机，分别是 5、6、6为后缀的IP

## 现在

固定IP算是设置完毕了，超简单，但是我墨迹了两天，边工作，边看看这个问题，算是一顶一废了。
