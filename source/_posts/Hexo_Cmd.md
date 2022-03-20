---
title: Hexo
date: 2021-11-19 14:25:09
tags: [Hexo]
excerpt: 本文章用来记录在github page部署Hexo的经过
categories: StorageBox
index_img: https://gitee.com/xlzf/blog-image/raw/master/Gongsi/Hexo_GitHub.jpeg
---

[文档 | Hexo](https://hexo.io/zh-cn/docs/)

[Hexo Fluid 用户手册 (fluid-dev.com)](https://hexo.fluid-dev.com/docs/)

## 安装`cnpm`

``` shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## 安装 `hexo-cli`

``` shell
cnpm install -g hexo-cli

cnpm install hexo --save
```

## 查看`hexo`版本

``` shell
hexo -v
```

## `hexo`初始化

``` shell
hexo init
```

## 安装套件

``` shell
cnpm install
```

## `hexo`安装`git`组件

```shell
npm install hexo-deployer-git --save
```

## 错误

1. ``` shell
   Please make sure you have the correct access rights
   and the repository exists.
   FATAL {
     err: Error: Spawn failed
         at ChildProcess.<anonymous> (E:\Hexo_New\node_modules\_hexo-util@2.6.0@hexo-util\lib\spawn.js:51:21)
         at ChildProcess.emit (node:events:390:28)
         at ChildProcess.cp.emit (E:\Hexo_New\node_modules\_cross-spawn@7.0.3@cross-spawn\lib\enoent.js:34:29)
         at Process.ChildProcess._handle.onexit (node:internal/child_process:290:12) {
       code: 128
     }
   } Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
   ```

   遇到上述问题，两个方面

   1. 没有生产rsa秘钥，这种好整，生成即可
   2. 生产了，而且是多个秘钥，命令找了找没有找到要找得，需要先执行debug `ssh -v git@github.com`,这里如果是gitee或者别的平台，可以改成`ssh -v git@gitee.com`,我这里是把本地 ~/.ssh/下生成的秘钥改成debug里要看到的秘钥即可，当然也可以告诉git，要走ssh下的哪个rsa key.
