---
title: C# 操作Oracle批量执行Insert Blob
date: 2021-11-04 22:08:55
tags: [Oracle]
excerpt: Oracle 批量 Insert Blob 操作
categories: C#
index_img: https://gitee.com/xlzf/blog-image/raw/master/Oracle.jpeg
---

# Oracle 批量 Insert Blob 操作

## 表

``` sql
create table BLOGTEST
(
  id      NUMBER not null,
  name    VARCHAR2(100),
  picture BLOB
)
```

## 序列

``` sql
create sequence SEQ_BlogTestID
minvalue 1
maxvalue 999999999999999999999999999
start with 1
increment by 1
cache 20;
```

## 类

``` csharp
namespace XLZFENTITY
{
    public class AddImage
    {
        public string Name { get; set; }

        public byte[] Image { get; set; }
    }
}
```

## 方法

``` csharp
/// <summary>
/// 增加一条数据
/// </summary>
public static int AddImage(string name, byte[] picture)
{
    StringBuilder strSql = new StringBuilder();

    strSql.Append(" INSERT INTO BLOGTEST( ");
    strSql.Append(" ID,NAME,PICTURE)");
    strSql.Append(" VALUES (");
    strSql.Append(" SEQ_BlogTestID.Nextval,:Name,:Picture) ");//SEQ_BlogTestID 是数据库中的序列

    int returnrow = 0;

    using (OracleConnection conn = new OracleConnection(connStr))
    {
        conn.Open();

        using (OracleCommand cmd = conn.CreateCommand())
        {
            cmd.CommandText = strSql.ToString();

            cmd.Parameters.Add(new OracleParameter(":Name", OracleDbType.Varchar2)).Value = name;

            cmd.Parameters.Add(new OracleParameter(":Picture", OracleDbType.Blob)).Value = picture;

            returnrow = cmd.ExecuteNonQuery();
        }
    }

    return returnrow;
}

/// <summary>
/// 示例：批量添加图片
/// </summary>
/// <param name="list"></param>
/// <returns></returns>
public static int AddImages(List<AddImage> list)
{
    StringBuilder strSql = new StringBuilder();

    strSql.Append(" INSERT INTO BLOGTEST( ");
    strSql.Append(" ID,NAME,PICTURE)");
    strSql.Append(" VALUES (");
    strSql.Append(" SEQ_BlogTestID.Nextval,:Name,:Picture) ");//SEQ_BlogTestID 是数据库中的序列

    int returnrow = 0;

    int recordCount = list.Count;

    using (OracleConnection conn = new OracleConnection(connStr))
    {
        conn.Open();

        using (OracleCommand cmd = conn.CreateCommand())
        {
            cmd.CommandText = strSql.ToString();

            cmd.ArrayBindCount = recordCount;  //指定单次需要处理的条数
            //注意，这里的值是数组
            cmd.Parameters.Add(new OracleParameter(":Name", OracleDbType.Varchar2, ParameterDirection.Input)).Value = list.Select(x => x.Name).ToArray(); //  item.Name;

            cmd.Parameters.Add(new OracleParameter(":Picture", OracleDbType.Blob, ParameterDirection.Input)).Value = list.Select(x => x.Image).ToArray();

            returnrow = cmd.ExecuteNonQuery();
        }
    }

    return returnrow;
}
```
