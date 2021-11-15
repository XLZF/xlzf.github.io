---
title: Docker for mysql
date: 2021-11-15 10:47:01
tags: [Docker, Mysql]
excerpt: windows docker 安装Mysql
categories: Mysql
index_img: https://gitee.com/xlzf/blog-image/raw/master/Gongsi/mysql.jpeg
---

# Docker 安装 Mysql

1. `docker pull mysql`

2. 先造一个容器

   `docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest --default-authentication-plugin=mysql_native_password`

3. 做映射

   提前做好路径：`D:\docker\mysql\conf`

   ![image-20211114161159024](https://gitee.com/xlzf/blog-image/raw/master/Home/20211114161203.png)

   `docker cp mysql:/etc/mysql/my.cnf D:/docker/mysql/conf/`

   ![image-20211114161305796](https://gitee.com/xlzf/blog-image/raw/master/Home/20211114161307.png)

4. 停止 `docker stop mysql`

5. 删除 `docker rm mysql`

6. 再造容器

   `docker run --name mysql -v d/docker/mysql/conf:/etc/mysql/conf.d -v d:/docker/mysql/data:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql`

7. 进入`mysql`

   `docker exec -it mysql mysql -uroot -p123456`

   报这个错的话，就稍等, 我就是等等就好了

   ```
   mysql: [Warning] World-writable config file '/etc/mysql/conf.d/my.cnf' is ignored.
   mysql: [Warning] Using a password on the command line interface can be insecure.
   ERROR 2002 (HY000): Can't cdocker exec -it mysql mysql -uroot -p123456 '/var/run/mysqld/mysqld.sock' (2)
   ```

8. 修改密码

   `alter user 'root'@'%' identified with mysql_native_password by 'Aa111111';`

9. 本地DBeaver 连接

   1. 服务器地址 127.0.0.1
   2. 端口 3306
   3. 数据库 空
   4. 用户名 root
   5. 密码 Aa111111
   
   ![image-20211114162007559](https://gitee.com/xlzf/blog-image/raw/master/Home/20211114162009.png)
