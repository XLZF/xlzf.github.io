---
title: Oralce Note
date: 2021-11-01 21:15:40
tags: [Oracle]
excerpt: 把工作中的关于Oralce的知识点记录到这。
categories: Oracle
index_img: https://gitee.com/xlzf/blog-image/raw/master/Oracle.jpeg
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


## Oracle SEQUENCE

​		序列(SEQUENCE)是序列号生成器，可以为表中的行自动生成序列号，产生一组等间隔的数值(类型为数字)。不占用磁盘空间，占用内存。

其主要用途是生成表的主键值，可以在插入语句中引用，也可以通过查询检查当前值，或使序列增至下一个值。

之前用的自增序列：

``` sql
create sequence SEQ_BlogTestID
minvalue 1
maxvalue 999999999999999999999999999
start with 1
increment by 1
cache 20;
```

基本语法：

``` sql
CREATE SEQUENCE 序列名

　　[INCREMENT BY n]

　　[START WITH n]

　　[{MAXVALUE/ MINVALUE n| NOMAXVALUE}]

　　[{CYCLE|NOCYCLE}]

　　[{CACHE n| NOCACHE}];
```

1.  INCREMENT BY用于定义序列的步长，如果省略，则默认为1，如果出现负值，则代表Oracle序列的值是按照此步长递减的。

2.  START WITH 定义序列的初始值(即产生的第一个值)，默认为1。

3.  MAXVALUE 定义序列生成器能产生的最大值。选项NOMAXVALUE是默认选项，代表没有最大值定义，这时对于递增Oracle序列，系统能够产生的最大值是10的27次方;对于递减序列，最大值是-1。

4.  MINVALUE定义序列生成器能产生的最小值。选项NOMAXVALUE是默认选项，代表没有最小值定义，这时对于递减序列，系统能够产生的最小值是?10的26次方;对于递增序列，最小值是1。

5. CYCLE和NOCYCLE 表示当序列生成器的值达到限制值后是否循环。CYCLE代表循环，NOCYCLE代表不循环。如果循环，则当递增序列达到最大值时，循环到最小值;对于递减序列达到最小值时，循环到最大值。如果不循环，达到限制值后，继续产生新值就会发生错误。

6. CACHE(缓冲)定义存放序列的内存块的大小，默认为20。NOCACHE表示不对序列进行内存缓冲。对序列进行内存缓冲，可以改善序列的性能。

   大量语句发生请求，申请序列时，为了避免序列在运用层实现序列而引起的性能瓶颈。Oracle序列允许将序列提前生成 cache x个先存入内存，在发生大量申请序列语句时，可直接到运行最快的内存中去得到序列。但cache个数也不能设置太大，因为在数据库重启时，会清空内存信息，预存在内存中的序列会丢失，当数据库再次启动后，序列从上次内存中最大的序列号+1 开始存入cache x个。这种情况也能会在数据库关闭时也会导致序号不连续。

7. NEXTVAL 返回序列中下一个有效的值，任何用户都可以引用。

8. CURRVAL 中存放序列的当前值,NEXTVAL 应在 CURRVAL 之前指定 ，二者应同时有效。



## TableSpace

``` sql
select a.tablespace_name tablespace_name
,nvl(ceil((1 - b.free / a.total) * 100), 100) "usage_of_tablespace%"
,nvl(b.free, 0) "left_space(M)"
,c.extent_management "Extent_management"
from (select tablespace_name, sum(nvl(bytes, 0)) / 1024 / 1024 total
from dba_data_files
group by tablespace_name) a
,(select tablespace_name, sum(nvl(bytes, 0)) / 1024 / 1024 free
from dba_free_space
group by tablespace_name) b
,dba_tablespaces c
where a.tablespace_name = c.tablespace_name
and c.tablespace_name = b.tablespace_name(+);


SELECT a.tablespace_name "表空间名",
       total "表空间大小",
       free "表空间剩余大小",
       (total - free) "表空间使用大小",
       total / (1024 * 1024 * 1024) "表空间大小(G)",
       free / (1024 * 1024 * 1024) "表空间剩余大小(G)",
       (total - free) / (1024 * 1024 * 1024) "表空间使用大小(G)",
       round((total - free) / total, 4) * 100 "使用率 %"
  FROM (SELECT tablespace_name, SUM(bytes) free
          FROM dba_free_space
         GROUP BY tablespace_name) a,
       (SELECT tablespace_name, SUM(bytes) total
          FROM dba_data_files
         GROUP BY tablespace_name) b 
 WHERE a.tablespace_name = b.tablespace_name;
```

