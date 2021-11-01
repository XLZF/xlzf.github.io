---
title: Oralce Note
date: 2021-11-01 21:15:40
tags: [Oralce]
excerpt: 把工作中的关于Oralce的知识点记录到这。
categories: Oralce
index_img: /img/Oracle.jpeg
---

## Oralce 锁表

1. 查询Oracle锁表

   ``` sql
   select sess.sid,
          sess.serial#,
          lo.oracle_username,
          lo.os_user_name,
          ao.object_name,
          lo.locked_mode
     from v$locked_object lo, dba_objects ao, v$session sess
    where ao.object_id = lo.object_id
      and lo.session_id = sess.sid;
   ```

2. 杀掉锁表进程

   ```sql
   alter system kill session '68,51';--分别为SID和SERIAL#号
   ```

## Oracle DBlink

1. 新建OracleDBlink 脚本

   ``` sql
   CREATE PUBLIC DATABASE LINK XX_DBLINK CONNECT TO 用户名（小写） IDENTIFIED BY 密码（小写） USING '(DESCRIPTION =
   (ADDRESS_LIST =
   (ADDRESS = (PROTOCOL = TCP)(HOST = 目标数据库IP)(PORT = 目标数据库端口))
   )
   (CONNECT_DATA =
   (SERVICE_NAME = 目标数据库服务名)
   )
   )'
   ```

   
