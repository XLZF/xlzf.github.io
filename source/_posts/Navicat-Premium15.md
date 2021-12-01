---
layout: posts
title: Navicat Premium 15
date: 2021-11-30 22:31:43
tags: [Program] 
excerpt: 记录安装过程
categories: StorageBox
index_img: https://gitee.com/xlzf/blog-image/raw/master/Home/20211201224654.jpeg
---
# `Navicat Premium 15`

![](https://gitee.com/xlzf/blog-image/raw/master/Home/20211201224654.jpeg)

## 前言

这几天一直想着把自己的电脑上Docker中把主流的数据库镜像都安装一遍，现在已经安装了

1. `Oracle-XE`
2. `MySql`
3. `Mssql`
3. `MongoDB`

以前一直用的是`PLSql`、`Dbeaver Community`, 考虑到`Dbeaver` 如果不用企业版也就是 `Dbeaver-ee` 连接`MongoDB`有点空难，所以考虑本篇说的软件`Navicat Premium 15`，主要记录怎么安装和[科技](http://www.downcc.com/soft/430673.html)。

## 安装

下载完正常安装即可。关闭所有安全软件，包括`windows安全中心`

重点

### 打开注册机

![打开注册机](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130232749.png)

### 选择路径

![选择路径](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130232926.png)

![稍等一下](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130233016.png)

### 核对信息

​	然后回到注册机，确保以下几个选项是对应的

- License为Ent[erp](http://www.downcc.com/k/erpapp/)rise
- Products为Premium
- Languages为Simplified Chinese（简体中文，其它语言版本请自选）
- Resale Version为Site license
- Your Name和Your Organization可以任意填写或者默认

![核对信息](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130233244.png)

### 生产注册码

上面几项设置好后，点击“Generate”，会自动生成一个注册码，如下图

![点击“Generate”](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130233350.png)

### 激活

这个时候就可以打开`navicat premium15`，将上面生成的注册码复制到注册窗口中（注册窗在头部“帮助”选项下面），点击激活，

![](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130233445.png)

会提示因为激活服务器暂时不可使用.....我们选择“手动激活”

![](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130233524.png)

会生成一个请求码

![](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130233829.png)

将请求码复制到注册机中的Request Code框中，然后点击Generate按钮

![](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130233900.png)

将上面的激活码复制到手动激活窗口中，并点击“激活”

![](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130233926.png)

这个时候就会弹出`Navicat` 现已激活

![](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130233955.png)

## 下载

扫描下述二维码，下载安装，仅供学习，资金充裕也希望充米买正版。

![扫描下载，仅供学习](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130231927.png)

## 避坑

连接`MSSQL`的时候需要执行安装目录的下的一个文件`msodbcsql_64.msi`,算是驱动。

## 连接测试

![Oracle](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130234625.png)

![Mysql](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130234723.png)

![MSSQL](https://gitee.com/xlzf/blog-image/raw/master/Home/20211130234750.png)

![MongoDB](https://gitee.com/xlzf/blog-image/raw/master/Home/20211201223653.png)

![Mysql|Oracle|Mssql|MongoDB](https://gitee.com/xlzf/blog-image/raw/master/Home/20211201223758.png)
