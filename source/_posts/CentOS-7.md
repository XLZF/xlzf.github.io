---
title: CentOS 7
date: 2021-10-30 17:23:05
tags: [Linux, CentOS7, Docker, Xshell]
excerpt: 一直玩Windows,尝试一下CentOs7解解渴。
categories: Linux
index_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/CentOS_VM.jpeg
banner_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/3.jpg
---

# CentOS7

## 安装

![image-20210813222017245](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222017245.png)

![image-20210813222157401](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222157401.png)

![image-20210813222256945](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222256945.png)

![image-20210813222533941](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222533941.png)

![image-20210813222637764](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222637764.png)

![image-20210813222707926](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222707926.png)

![image-20210813222736374](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222736374.png)

![image-20210813222759917](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222759917.png)

![image-20210813222820140](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222820140.png)

![image-20210813222948110](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222948110.png)

![image-20210813222921036](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813222921036.png)

![image-20210813223019158](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813223019158.png)

![image-20210813223036473](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813223036473.png)

![image-20210813223055113](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813223055113.png)

![image-20210813223122945](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813223122945.png)

![image-20210813223141978](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813223141978.png)

![image-20210813224259862](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224259862.png)

![image-20210813224432142](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224432142.png)

![image-20210813224506500](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224506500.png)

![image-20210813224537039](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224537039.png)

![image-20210813224615048](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224615048.png)

![image-20210813224632445](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224632445.png)

![image-20210813224713441](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224713441.png)

![image-20210813224757244](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224757244.png)

![](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224815295.png)

![image-20210813224845477](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224845477.png)

![image-20210813224907724](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224907724.png)

![image-20210813224930022](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224930022.png)

![image-20210813224948885](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813224948885.png)

![image-20210813225019577](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813225019577.png)

<img src="https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210813225122432.png" alt="image-20210813225122432" style="zoom:80%;" />

## XShell 连接

### 先在虚拟机中查看IP

`ifconfig`

![image-20210814151123899](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814151123899.png)

### xshell 新建连接

![image-20210814151253098](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814151253098.png)

![image-20210814151518393](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814151518393.png)

![image-20210814151543205](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814151543205.png)

![image-20210814151612377](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814151612377.png)

## 基础操作

### 配置IP

`vi /etc/sysconfig/network-scripts/ifcfg-ens33`

灵活配置

``` shell
TYPE="Ethernet"   # 网络类型为以太网
BOOTPROTO="static"  # 手动分配ip
NAME="ens33"  # 网卡设备名，设备名一定要跟文件名一致
DEVICE="ens33"  # 网卡设备名，设备名一定要跟文件名一致
ONBOOT="yes"  # 该网卡是否随网络服务启动
IPADDR="192.168.220.101"  # 该网卡ip地址就是你要配置的固定IP，如果你要用xshell等工具连接，220这个网段最好和你自己的电脑网段一致，否则有可能用xshell连接失败
GATEWAY="192.168.220.2"   # 网关
NETMASK="255.255.255.0"   # 子网掩码
DNS1="8.8.8.8"    # DNS，8.8.8.8为Google提供的免费DNS服务器的IP地址
```

### 配置网络工作

`vi /etc/sysconfig/network`

``` shell
NETWORKING=yes # 网络是否工作，此处一定不能为no
```

### 配置公共DNS服务(可选)

`vi /etc/resolv.conf`

``` shell
nameserver 8.8.8.8
```

### 关闭防火墙

``` shell
systemctl stop firewalld # 临时关闭防火墙
systemctl disable firewalld # 禁止开机启动
```

### 重启网络服务

``` shell
service network restart
```

### 重置root 密码

安装完CentOs7之后有的时候会获取不到root的权限，需要su root ，这个时候忘记密码，需要重置密码，需要以下步骤。

#### 进入编辑界面

重启CentOs,到如下界面，快速按 e 键 进入编辑界面

![image-20210731215547837](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210731215547837.png)

![image-20210731215723728](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210731215723728.png)

#### 编辑

按方向键下键`↓`，找到设置语言的地方，如`LANG=en_US.UTF-8`，在后面追加`rw single init=/bin/bash`,然后按`ctrl+x`重启系统

![image-20210731215809612](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210731215809612.png)

#### 进入bash界面

进入bash界面后，可以输入`passwd`命令重新设置root密码

![image-20210731215953541](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210731215953541.png)

#### 剩下操作

* 如果开启了SELinux，执行命令`touch /.autorelabel`命令
* 输入`exec /sbin/init`命令重启系统

## 安裝FTP

### 查看是否安装ftp

`rpm -qa | grep vsftpd`

### 如果现实ftp，则进行下面命令卸载

`rpm -e  vsftpd-*`

### 使用以下命令安装Ftp(在线)

``` shell
#安装
yum install -y vsftpd
```

### 配置FTP

默认安装配置文件流路径为：/etc/vsftpd/vsftpd.conf

然后我们打开该配置文件 vi vsftpd.conf

修改内容如下：

``` shell
# 第一处 12行左右
# Allow anonymous FTP? (Beware - allowed by default if you comment this out).--是否允许匿名访问
anonymous_enable=NO

#第二处  33行左右
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.  -- 匿名用户登录否创建新目录。
anon_mkdir_write_enable=YES

#第三处  48行左右
# If you want, you can arrange for uploaded anonymous files to be owned by
chown_uploads=YES

#第四处 修改72行   异步中断启用？？
async_abor_enable=YES

#第五处 修改83行
ascii_upload_enable=YES

#第六处 修改84行
ascii_download_enable=YES

#第七处 修改87行
ftpd_banner=Welcome to blah FTP service.

#第八处 修改101行   --是否将所有用户限制在主目录,YES为启用 NO禁用.
hroot_local_user=YES

# 然后再文件的最后添加如下配置：

# 使用本地时间
use_localtime=YES
# 监听端口
listen_port=21
# 空闲回话超时时间 单位秒
idle_session_timeout=300
# 来宾启用 
guest_enable=YES
# 来宾用户名称
guest_username=vsftpd
# 用户配置文件
user_config_dir=/etc/vsftpd/uconf
# 连接超时时间  单位秒
data_connection_timeout=100
# ？？？
virtual_use_local_privs=YES
# pasv模式下指定端口范围 最小和最大
pasv_min_port=40000
pasv_max_port=40010
# 接受超时 次数？
accept_timeout=5
# 连接超时 单位秒
connect_timeout=100
# 允许写入根目录
allow_writeable_chroot=YES
```

先按照上述配置

配置完成品如下：

``` shell
# Example config file /etc/vsftpd/vsftpd.conf
#
# The default compiled in settings are fairly paranoid. This sample file
# loosens things up a bit, to make the ftp daemon more usable.
# Please see vsftpd.conf.5 for all compiled in defaults.
#
# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
# capabilities.
#
# Allow anonymous FTP? (Beware - allowed by default if you comment this out).
anonymous_enable=NO
#
# Uncomment this to allow local users to log in.
# When SELinux is enforcing check for SE bool ftp_home_dir
local_enable=YES
#
# Uncomment this to enable any form of FTP write command.
write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
# When SELinux is enforcing check for SE bool allow_ftpd_anon_write, allow_ftpd_full_access
#anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/xferlog
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode. The vsftpd.conf(5) man page explains
# the behaviour when these options are disabled.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
ascii_upload_enable=YES
ascii_download_enable=YES
#
# You may fully customise the login banner string:
ftpd_banner=Welcome to blah FTP service.
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd/banned_emails
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
chroot_local_user=YES
#chroot_list_enable=YES
# (default follows)
#chroot_list_file=/etc/vsftpd/chroot_list
#
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
#
# When "listen" directive is enabled, vsftpd runs in standalone mode and
# listens on IPv4 sockets. This directive cannot be used in conjunction
# with the listen_ipv6 directive.
listen=NO
#
# This directive enables listening on IPv6 sockets. By default, listening
# on the IPv6 "any" address (::) will accept connections from both IPv6
# and IPv4 clients. It is not necessary to listen on *both* IPv4 and IPv6
# sockets. If you want that (perhaps because you want to listen on specific
# addresses) then you must run two copies of vsftpd with two configuration
# files.
# Make sure, that one of the listen options is commented !!
listen_ipv6=YES

pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES

# 使用本地时间
use_localtime=YES
# 监听端口
listen_port=21
# 空闲回话超时时间 单位秒
idle_session_timeout=300
# 来宾启用 
guest_enable=YES
# 来宾用户名称
guest_username=vsftpd
# 用户配置文件
user_config_dir=/etc/vsftpd/uconf
# 连接超时时间  单位秒
data_connection_timeout=100
# ？？？
virtual_use_local_privs=YES
# pasv模式下指定端口范围 最小和最大
pasv_min_port=40000
pasv_max_port=40010
# 接受超时 次数？
accept_timeout=5
# 连接超时 单位秒
connect_timeout=100
# 允许写入根目录
allow_writeable_chroot=YES
```

### 建立用户文件并生成 并 生成用户数据文件

``` shell
#创建编辑用户文件
vim /etc/vsftpd/ftpuser
#第一行为用户名，第二行为密码。不能使用root作为用户名 

houshuai
houshuai
```

``` shell
db_load -T -t hash -f /etc/vsftpd/ftpuser /etc/vsftpd/ftpuser.db

#设定PAM验证文件，并指定对虚拟用户数据库文件进行读取

chmod 600 /etc/vsftpd/ftpuser.db 
```

### 修改 /etc/pam.d/vsftpd 文件

这里没有整明白是啥事，没有跟风，后续有机会再研究

``` shell
# 修改前先备份 

cp /etc/pam.d/vsftpd /etc/pam.d/vsftpd.bak

vi /etc/pam.d/vsftpd
#先将配置文件中原有的 auth 及 account 的所有配置行均注释掉
auth sufficient /lib64/security/pam_userdb.so db=/etc/vsftpd/ftpuser
account sufficient /lib64/security/pam_userdb.so db=/etc/vsftpd/ftpuser

# 如果系统为32位，上面改为lib
```

### 新建系统用户vsftpd，用户目录为/home/vsftpd

``` shell
#用户登录终端设为/bin/false(即：使之不能登录系统)
useradd vsftpd -d /home/vsftpd -s /bin/false
chown -R vsftpd:vsftpd /home/vsftpd
```

### 建立虚拟用户个人配置文件

``` shell
mkdir /etc/vsftpd/uconf
cd /etc/vsftpd/uconf

#这里建立虚拟用户wanghao配置文件
touch houshuai

#编辑wanghao用户配置文件，内容如下，其他用户类似
vi houshuai

local_root=/home/vsftpd/houshuai/
write_enable=YES
anon_world_readable_only=NO
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES

#建立wanghao用户根目录
mkdir -p /home/vsftpd/houshuai/
```

然后我们在/home/vsftpd/houshuai 下面创建一个index.html 测试文件

### 防火墙设置

``` shell
# IPtables 的设置方式：
vi /etc/sysconfig/iptables
#编辑iptables文件，添加如下内容，开启21端口
-A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 40000:40010 -j ACCEPT


# firewall 的设置方式：
firewall-cmd --zone=public --add-service=ftp --permanent

firewall-cmd --zone=public --add-port=21/tcp --permanent
firewall-cmd --zone=public --add-port=40000-40010/tcp --permanent
```

### 启动ftp服务器

``` shell
#设置开机启动
systemctl enable vsftpd.service

#启动
systemctl start vsftpd.service

#停止
systemctl stop vsftpd.service

#重启
systemctl restart vsftpd.service

#查看状态
systemctl status vsftpd.service另一种service vsftpd restartservice vsftpd status
```

### 连接

![image-20210814184803023](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814184803023.png)

![image-20210814190436082](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814190436082.png)

![image-20210814190456161](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814190456161.png)

### 问题

1. 此间会遇到一些问题，诸如：503 等等，就是防火墙关了就行 `systemctl stop firewalld`，具体为什么，防火墙端口的活。

2. `553 Could not create file` 问题，网友们说的有点纷杂，整理一下

   1. SeLinux 需要关闭 `vim /etc/selinux/config` 设置 `SELINUX=disabled`   设置完还是没有解决，可能是没有重启的原因。

   2. 查了一下，应该是共享的文件夹权限的问题，手动设置，有点懒

      ![image-20210814190147729](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814190147729.png)

      ![image-20210814190230363](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814190230363.png)

      ![image-20210814190345335](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20210814190345335.png)

      

## 安装Docker

### 日志

``` shell
[root@centOs7 ~]# uname -a
Linux centOs7 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
[root@centOs7 ~]# yum update
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.bfsu.edu.cn
 * extras: mirrors.huaweicloud.com
 * updates: mirrors.huaweicloud.com
base                                                     | 3.6 kB     00:00     
extras                                                   | 2.9 kB     00:00     
updates                                                  | 2.9 kB     00:00     
(1/4): base/7/x86_64/group_gz                              | 153 kB   00:00     
(2/4): extras/7/x86_64/primary_db                          | 242 kB   00:00     
(3/4): updates/7/x86_64/primary_db                         | 9.5 MB   00:00     
(4/4): base/7/x86_64/primary_db                            | 6.1 MB   00:02     
正在解决依赖关系
--> 正在检查事务
---> 软件包 NetworkManager.x86_64.1.1.18.8-1.el7 将被 升级
---> 软件包 NetworkManager.x86_64.1.1.18.8-2.el7_9 将被 更新
---> 软件包 NetworkManager-adsl.x86_64.1.1.18.8-1.el7 将被 升级
...
---> 软件包 zlib.x86_64.0.1.2.7-19.el7_9 将被 更新
updates/7/x86_64/filelists_db                            | 5.5 MB     00:00     
--> 解决依赖关系完成

依赖关系解决

================================================================================
 Package                          架构   版本                     源       大小
================================================================================
正在安装:
 kernel                           x86_64 3.10.0-1160.36.2.el7     updates  50 M
正在更新:
 NetworkManager                   x86_64 1:1.18.8-2.el7_9         updates 1.9 M
 NetworkManager-adsl              x86_64 1:1.18.8-2.el7_9         updates 163 k
 NetworkManager-glib              x86_64 1:1.18.8-2.el7_9         updates 1.5 M
 NetworkManager-libnm             x86_64 1:1.18.8-2.el7_9         updates 1.7 M
 ...
 xorg-x11-server-common           x86_64 1.20.4-16.el7_9          updates  56 k
 zlib                             x86_64 1.2.7-19.el7_9           updates  90 k

事务概要
================================================================================
安装    1 软件包
升级  236 软件包

总下载量：487 M
Is this ok [y/d/N]: y
Downloading packages:
No Presto metadata available for updates
警告：/var/cache/yum/x86_64/7/updates/packages/NetworkManager-adsl-1.18.8-2.el7_9.x86_64.rpm: 头V3 RSA/SHA256 Signature, 密钥 ID f4a80eb5: NOKEY
NetworkManager-adsl-1.18.8-2.el7_9.x86_64.rpm 的公钥尚未安装
(1/237): NetworkManager-adsl-1.18.8-2.el7_9.x86_64.rpm     | 163 kB   00:00     
(2/237): NetworkManager-1.18.8-2.el7_9.x86_64.rpm          | 1.9 MB   00:00     
(3/237): NetworkManager-libnm-1.18.8-2.el7_9.x86_64.rpm    | 1.7 MB   00:00     
...
(237/237): xorg-x11-server-Xorg-1.20.4-16.el7_9.x86_64.rpm | 1.5 MB   00:00     
--------------------------------------------------------------------------------
总计                                                21 MB/s | 487 MB  00:23     
从 file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 检索密钥
导入 GPG key 0xF4A80EB5:
 用户ID     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
 指纹       : 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
 软件包     : centos-release-7-9.2009.0.el7.centos.x86_64 (@anaconda)
 来自       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
是否继续？[y/N]：y
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在更新    : 1:grub2-common-2.02-0.87.el7.centos.6.noarch              1/473 
  正在更新    : centos-release-7-9.2009.1.el7.centos.x86_64               2/473 
  ...
  正在更新    : iwl4965-firmware-228.61.2.24-80.el7_9.noarch            237/473 
  清理        : firefox-68.10.0-1.el7.centos.x86_64                     238/473 
  清理        : 12:dhclient-4.2.5-82.el7.centos.x86_64                  239/473 
  ...
  清理        : tzdata-2020a-1.el7.noarch                               473/473 
  验证中      : 7:device-mapper-event-1.02.170-6.el7_9.5.x86_64           1/473 
  验证中      : kernel-tools-libs-3.10.0-1160.36.2.el7.x86_64             2/473 
  ...
  验证中      : pulseaudio-module-bluetooth-10.0-5.el7.x86_64           472/473 
  验证中      : libsmartcols-2.23.2-65.el7.x86_64                       473/473 

已安装:
  kernel.x86_64 0:3.10.0-1160.36.2.el7        
更新完毕:
  NetworkManager.x86_64 1:1.18.8-2.el7_9                                        
  NetworkManager-adsl.x86_64 1:1.18.8-2.el7_9    
  ...
  xorg-x11-server-Xorg.x86_64 0:1.20.4-16.el7_9                                 
  xorg-x11-server-common.x86_64 0:1.20.4-16.el7_9                               
  zlib.x86_64 0:1.2.7-19.el7_9                                                  

完毕！
[root@centOs7 ~]# yum install -y yum-utils device-mapper-persistent-data lvm2
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.bfsu.edu.cn
 * extras: mirrors.bfsu.edu.cn
 * updates: mirrors.huaweicloud.com
软件包 yum-utils-1.1.31-54.el7_8.noarch 已安装并且是最新版本
软件包 device-mapper-persistent-data-0.8.5-3.el7_9.2.x86_64 已安装并且是最新版本
软件包 7:lvm2-2.02.187-6.el7_9.5.x86_64 已安装并且是最新版本
无须任何处理
[root@centOs7 ~]# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
已加载插件：fastestmirror, langpacks
adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
grabbing file https://download.docker.com/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo
[root@centOs7 ~]# yum list docker-ce --showduplicates | sort -r
已加载插件：fastestmirror, langpacks
可安装的软件包
 * updates: mirrors.huaweicloud.com
Loading mirror speeds from cached hostfile
 * extras: mirrors.huaweicloud.com
docker-ce.x86_64            3:20.10.7-3.el7                     docker-ce-stable
docker-ce.x86_64            3:20.10.6-3.el7                     docker-ce-stable
...
docker-ce.x86_64            17.03.0.ce-1.el7.centos             docker-ce-stable
 * base: mirrors.bfsu.edu.cn
[root@centOs7 ~]# yum install docker-ce-17.12.1.ce
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.bfsu.edu.cn
 * extras: mirrors.tuna.tsinghua.edu.cn
 * updates: mirrors.tuna.tsinghua.edu.cn
正在解决依赖关系
--> 正在检查事务
---> 软件包 docker-ce.x86_64.0.17.12.1.ce-1.el7.centos 将被 安装
--> 正在处理依赖关系 container-selinux >= 2.9，它被软件包 docker-ce-17.12.1.ce-1.el7.centos.x86_64 需要
--> 正在检查事务
---> 软件包 container-selinux.noarch.2.2.119.2-1.911c772.el7_8 将被 安装
--> 解决依赖关系完成

依赖关系解决

================================================================================
 Package            架构    版本                        源                 大小
================================================================================
正在安装:
 docker-ce          x86_64  17.12.1.ce-1.el7.centos     docker-ce-stable   30 M
为依赖而安装:
 container-selinux  noarch  2:2.119.2-1.911c772.el7_8   extras             40 k

事务概要
================================================================================
安装  1 软件包 (+1 依赖软件包)

总下载量：30 M
安装大小：30 M
Is this ok [y/d/N]: y
Downloading packages:
(1/2): container-selinux-2.119.2-1.911c772.el7_8.noarch.rp |  40 kB   00:00     
warning: /var/cache/yum/x86_64/7/docker-ce-stable/packages/docker-ce-17.12.1.ce-1.el7.centos.x86_64.rpm: Header V4 RSA/SHA512 Signature, key ID 621e9f35: NOKEY
docker-ce-17.12.1.ce-1.el7.centos.x86_64.rpm 的公钥尚未安装
(2/2): docker-ce-17.12.1.ce-1.el7.centos.x86_64.rpm        |  30 MB   06:51     
--------------------------------------------------------------------------------
总计                                                76 kB/s |  30 MB  06:52     
从 https://download.docker.com/linux/centos/gpg 检索密钥
导入 GPG key 0x621E9F35:
 用户ID     : "Docker Release (CE rpm) <docker@docker.com>"
 指纹       : 060a 61c5 1b55 8a7f 742b 77aa c52f eb6b 621e 9f35
 来自       : https://download.docker.com/linux/centos/gpg
是否继续？[y/N]：y
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : 2:container-selinux-2.119.2-1.911c772.el7_8.noarch          1/2 
  正在安装    : docker-ce-17.12.1.ce-1.el7.centos.x86_64                    2/2 
  验证中      : docker-ce-17.12.1.ce-1.el7.centos.x86_64                    1/2 
  验证中      : 2:container-selinux-2.119.2-1.911c772.el7_8.noarch          2/2 

已安装:
  docker-ce.x86_64 0:17.12.1.ce-1.el7.centos                                    

作为依赖被安装:
  container-selinux.noarch 2:2.119.2-1.911c772.el7_8                            

完毕！
[root@centOs7 ~]# systemctl start docker
[root@centOs7 ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@centOs7 ~]# docker version
Client:
 Version:	17.12.1-ce
 API version:	1.35
 Go version:	go1.9.4
 Git commit:	7390fc6
 Built:	Tue Feb 27 22:15:20 2018
 OS/Arch:	linux/amd64

Server:
 Engine:
  Version:	17.12.1-ce
  API version:	1.35 (minimum version 1.12)
  Go version:	go1.9.4
  Git commit:	7390fc6
  Built:	Tue Feb 27 22:17:54 2018
  OS/Arch:	linux/amd64
  Experimental:	false
[root@centOs7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@centOs7 ~]# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
b8dfde127a29: Pull complete 
Digest: sha256:df5f5184104426b65967e016ff2ac0bfcd44ad7899ca3bbcf8e44e4461491a9e
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

[root@centOs7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@centOs7 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              d1165f221234        4 months ago        13.3kB
[root@centOs7 ~]# abrt-cli list --since 1627769906
```

### 日志中关键命令

* root账户登录，查看内核版本如下:

  ``` shell
  [root@localhost ~]# uname -a
  ```

* （可选）把yum包更新到最新

    **(生产环境慎重！yum update会对软件包和内核升级，此处只是为了排除系统环境的影响，来自笔者的备注—2019年10月30日**)

  ``` shell
  [root@localhost ~]# yum update
  ```

* 安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

  ```  shell
  [root@localhost ~]# yum install -y yum-utils device-mapper-persistent-data lvm2
  ```

* 设置yum源

  ``` shell
  [root@localhost ~]# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
  ```

* 可以查看所有仓库中所有docker版本，并选择特定版本安装

  ``` shell
  [root@localhost ~]# yum list docker-ce --showduplicates | sort -r
  ```

* 安装Docker，命令：yum install docker-ce-版本号，我选的是17.12.1.ce，如下

  ``` shell
  [root@localhost ~]# yum install docker-ce-17.12.1.ce
  ```

* 启动Docker，命令：systemctl start docker，然后加入开机启动，如下

  ``` shell
  [root@localhost ~]# systemctl start docker
  [root@localhost ~]# systemctl enable docker
  Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
  ```

* 验证安装是否成功(有client和service两部分表示docker安装启动都成功了)

  ``` shell
  [root@localhost ~]# docker version 
  ```

### Docker 常用命令

* docker ps 查看当前正在运行的容器

* docker ps -a 查看所有容器的状态

* docker start/stop id/name 启动/停止某个容器

* docker attach id 进入某个容器(使用exit退出后容器也跟着停止运行)

* docker exec -ti id 启动一个伪终端以交互式的方式进入某个容器（使用exit退出后容器不停止运行）

* docker images 查看本地镜像

* docker rm id/name 删除某个容器

* docker rmi id/name 删除某个镜像

* docker run --name test -ti ubuntu /bin/bash 

  复制ubuntu容器并且重命名为test且运行，然后以伪终端交互式方式进入容器，运行bash

* docker build -t soar/centos:7.1 . 通过当前目录下的Dockerfile创建一个名为soar/centos:7.1的镜像

* docker run -d -p 2222:22 --name test soar/centos:7.1 

  以镜像soar/centos:7.1创建名为test的容器，并以后台模式运行，并做端口映射到宿主机2222端口，P参数重启容器宿主机端口会发生改变