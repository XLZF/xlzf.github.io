---
title: Git 常用命令及坑记录
date: 2021-11-19 14:25:09
tags: [Git]
excerpt: 本文章用来记录Git常用命令和坑
categories: StorageBox
index_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/0026119fded9dc031f1cdc028cd5b535.jpeg
---

## 配置用户名及邮箱

`git config`可以配置git的参数，可以使用`git config --list`查看已经配置的git参数。

其中有三个级别的保存位置，`--system`、`--global`、`--local`，分别

- 表示所有用户（本系统）
- 当前用户（全局）
- 本地配置（当前目录）

默认使用`--local`


```shell
git config --global user.name "houshuai" 
git config --global user.email "houshuai@bjgoodwill.com"
```

## 获取远程仓库

``` shell
git clone git@git.com
```

## 生成SSH秘钥

``` shell
ssh-keygen -t rsa -C "hou3125378@live.com"
```

## 常见问题

### 没有生成`SSHkey`或者找不到对应的 `SSHkey`

``` shell
Permission denied(publickey).
fatal: Could not read from remote repository.
Please make sure you have the correct access right 
and the repository exists
```

解决：

1. 生成`SSHKey`

   ``` shell
   ssh-keygen -t rsa -C "XXX@qq.com"
   ```

2. debug

   ``` shell
   ssh -v git@github.com
   ```

3. agent

   ``` shell
   ssh-agent -s
   ```

4. 将对应的`SSHKey`

   ``` shell
   ssh-add ~/.ssh/id_rsa
   ```

​		到这里如果遇到这个问题

​		`Could not open a connection to your authentication agent`

5. 啥意思我也不知道

   ``` shell
   eval `ssh-agent -s`
   ```

6. 重新指向

   ``` shell
   ssh-add ~/.ssh/id_rsa
   ```

​	自己遇到这个问题不是没生成`sshkey`,而是多个`sshkey` 没有对应好

​	参考地址:[Git报错解决：git@gitee.com: Permission denied (publickey). - 尚码园 (shangmayuan.com)](https://www.shangmayuan.com/a/39405a3fee8f4ce0aca1bc39.html)

