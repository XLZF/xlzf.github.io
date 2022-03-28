---
title: SQL Sugar
date: 2021-11-01 22:24:43
tags: [Linq]
excerpt: 几年前看大神文章留的笔记。
categories: C#
index_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/Csharp.jpeg
---

## [SqlSugar 学习](http://www.codeisbug.com/Doc/8/1166)

### 记录

### 1.  多表查询

三表查询

``` csharp
int total = 0;
var list = db.Queryable<SYS_User_UserGroup, SYS_User, SYS_UserGroup>((a, b, c) => new object[]
{
    JoinType.Left,a.User_ID == b.User_ID,
    JoinType.Left,c.UserGroup_ID == a.UserGroup_ID
})
.Select((a, b, c) => new { a.User_ID, b.User_Name, c.UserGroup_Name });
```
### 动态where

``` csharp
List<IConditionalModel> conModels = new List<IConditionalModel>
{
    new ConditionalModel() { FieldName = "", ConditionalType = ConditionalType.Like, FieldValue = "" }
};

var student = db.Queryable<SYS_User>().Where(conModels).ToList();
```
###  TOP

``` c#
var top5 = db.Queryable<SYS_User>().Take(5).ToList();
```
### 根据主键查询

``` c#
var getByPrimaryKey = db.Queryable<SYS_User>().InSingle(2);
```
### 查询单条没有数据返回NULL, Single超过1条会报错，First不会

``` csharp
var getSingleOrDefault = db.Queryable<SYS_User>().Single();
var getFirstOrDefault = db.Queryable<SYS_User>().First();
/*
生成SQL: 参考Take函数
如果使用Single方法返回单条, 实际返回超过1条, 会引发异常, 使用First返回单条不会引发异常, 只返回第一条, 忽略其它的结果.
*/
```
### UNION ALL
``` csharp
db.UnionAll<SYS_User>(db.Queryable<SYS_User>(),db.Queryable<SYS_User>()).ToList();
/*
生成SQL:
SQL Server:
SELECT * FROM  (SELECT [ID],[Name] FROM [Student] UNION ALL SELECT [ID],[Name] FROM [Student] ) unionTable  

MySQL:
SELECT * FROM  (SELECT `ID`,`Name` FROM `Student`  UNION ALL SELECT `ID`,`Name` FROM `Student`  ) unionTable   

Oracle:
SELECT * FROM  (SELECT "ID","NAME" FROM "STUDENT" UNION ALL SELECT "ID","NAME" FROM "STUDENT" ) UNIONTABLE  

Sqlite:
SELECT * FROM  (SELECT `ID`,`Name` FROM `Student`  UNION ALL SELECT `ID`,`Name` FROM `Student`  ) unionTable   
*/
```
### IN查询
``` csharp
//Id In (1,2,3), 指定列In查询
var in1 = db.Queryable<SYS_User>().In(it=>it.ID,new int[] { 1, 2, 3 }).ToList();
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [SYS_User]  WHERE [ID] IN ('1','2','3')  
其它数据库类似, 就不一一举例了
*/

//主键 In (1,2,3)  不指定列, 默认根据主键In
var in2 = db.Queryable<SYS_User>().In(new int[] { 1, 2, 3 }).ToList();
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [SYS_User]  WHERE [ID] IN ('1','2','3')  
其它数据库类似, 就不一一举例了
*/

//Id In (1,2)  指定列的另外一种写法
int[] array = new int[] { 1, 2 };
var in3 = db.Queryable<SYS_User>().Where(it=>array.Contains(it.ID)).ToList();
/*
生成SQL和上面一样
*/
```
### NOT IN查询
``` csharp
var in3 = db.Queryable<SYS_User>().Where(it=>!array.Contains(it.ID)).ToList();
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [SYS_User]  WHERE NOT ([ID] IN ('1','2')) 
其它数据库类似, 就不一一举例了
*/
```
###  条件查询
``` csharp
var getByWhere = db.Queryable<SYS_User>().Where(it => it.Id == 1 || it.Name == "a").ToList();
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [SYS_User]  WHERE (( [ID] = @ID0 ) OR ( [Name] = @Name1 ))
其中@ID0值为1, @Name1值为a

MySQL:
SELECT `ID`,`Name` FROM `SYS_User`  WHERE (( `ID` = @ID0 ) OR ( `Name` = @Name1 )) 
其中@ID0值为1, @Name1值为a

Oracle:
SELECT "ID","NAME" FROM "SYS_User"  WHERE (( "ID" = :ID0 ) OR ( "NAME" = :Name1 ))
其中:ID0值为1, :Name1值为a

Sqlite:
SELECT `ID`,`Name` FROM `SYS_User`  WHERE (( `ID` = @ID0 ) OR ( `Name` = @Name1 )) 
其中@ID0值为1, @Name1值为a
*/
```
### 使用函数 SqlFunc类
``` csharp
//查询Name列不为空的结果, SqlFunc提供的功能远不止这一个, 在查询函数里面有详解
var getByFuns = db.Queryable<SYS_User>().Where(it => SqlFunc.IsNullOrEmpty(it.Name)).ToList();
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [SYS_User]  WHERE ( [Name]='' OR [Name] IS NULL )

MySQL:
SELECT `ID`,`Name` FROM `SYS_User`  WHERE ( `Name`='' OR `Name` IS NULL ) 

Oracle:
SELECT "ID","NAME" FROM "SYS_User"  WHERE ( "NAME"='' OR "NAME" IS NULL )

Sqlite:
SELECT `ID`,`Name` FROM `SYS_User`  WHERE ( `Name`='' OR `Name` IS NULL ) 
*/
```
### 可以使用 SUM MAX MIN AVG查询单个字段
``` csharp
var sum = db.Queryable<SYS_User>().Sum(it => it.ID);
/*
生成SQL:
SQL Server:
SELECT SUM([ID]) FROM [SYS_User]

其它数据库类型, 不一一列举
*/
```
### Between 1 and 20
``` csharp
var between = db.Queryable<SYS_User>().Where(it => SqlFunc.Between(it.ID, 1, 20)).ToList();
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [SYS_User]  WHERE  ([ID] BETWEEN @MethodConst0 AND @MethodConst1) 
其中@MethodConst0值为1, @MethodConst1值为20

MySQL:
SELECT `ID`,`Name` FROM `SYS_User`  WHERE  (`ID` BETWEEN @MethodConst0 AND @MethodConst1)  
其中@MethodConst0值为1, @MethodConst1值为20

Oracle:
SELECT "ID","NAME" FROM "SYS_User"  WHERE  ("ID" BETWEEN :MethodConst0 AND :MethodConst1) 
其中:MethodConst0值为1, :MethodConst1值为20

Sqlite:
SELECT `ID`,`Name` FROM `SYS_User`  WHERE  (`ID` BETWEEN @MethodConst0 AND @MethodConst1)  
其中@MethodConst0值为1, @MethodConst1值为20
*/
```
### 使用 AS 取新的表名
``` csharp
var getListByRename = db.Queryable<School>().AS("Student").ToList();
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [Student] 
*/
```
### 排序
``` csharp
var getAllOrder = db.Queryable<SYS_User>().OrderBy(it => it.ID).ToList(); //默认为ASC排序
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [SYS_User] ORDER BY [ID] ASC

其它数据库类似
*/

var getAllOrder = db.Queryable<SYS_User>().OrderBy(it => it.ID, OrderByType.Desc).ToList(); //收到设置为DESC排序
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [SYS_User] ORDER BY [ID] DESC
*/
```
###  多个字段排序
``` csharp
var data = db.Queryable<SYS_User>()
    .OrderBy(it => it.ID, OrderByType.Desc)
    .OrderBy(it => it.Name, OrderByType.Asc)
    .ToList();
/*
生成SQL:
SQL Server:
SELECT [ID],[Name] FROM [SYS_User] ORDER BY [ID] DESC,[Name] ASC

其它数据库类似
*/
```
### 是否存在这条记录
``` csharp
var isAny = db.Queryable<SYS_User>().Where(it => it.Id == -1).Any();
var isAny2 = db.Queryable<SYS_User>().Any(it => it.Id == -1);
```
### 获取同一天的记录
``` csharp
var getTodayList = db.Queryable<SYS_User>().Where(it => SqlFunc.DateIsSame(it.CreateTime, DateTime.Now)).ToList();
/*
生成SQL:
SQL Server:
SELECT [ID],[Name],[CreateTime] FROM [Student]  WHERE  (DATEDIFF(day,[CreateTime],@MethodConst0)=0) 
其中@MethodConst0值为当前时间

MySQL:
SELECT `ID`,`Name`,`CreateTime` FROM `Student`  WHERE  (TIMESTAMPDIFF(day,`CreateTime`,@MethodConst0)=0)  
其中@MethodConst0值为当前时间
*/
```

### whereType
``` csharp
// and id=100 and (id=1 or id=2 and id=1) 
List<IConditionalModel> conModels = new List<IConditionalModel>();
conModels.Add(new ConditionalModel() { FieldName = "id", ConditionalType = ConditionalType.Equal, FieldValue = "100" });
conModels.Add(new ConditionalCollections()
{
    ConditionalList =
    new List<KeyValuePair<WhereType, ConditionalModel>>()
    {
        new  KeyValuePair<WhereType, ConditionalModel>( WhereType.And ,new ConditionalModel() { FieldName = "id", ConditionalType = ConditionalType.Equal, FieldValue = "1" }),
        new  KeyValuePair<WhereType, ConditionalModel>( WhereType.Or, new ConditionalModel() { FieldName = "id", ConditionalType = ConditionalType.Equal, FieldValue = "2" }),
        new  KeyValuePair<WhereType, ConditionalModel>( WhereType.And,new ConditionalModel() { FieldName = "id", ConditionalType = ConditionalType.Equal, FieldValue = "2" })
    }
});
var student = Db.Queryable<SYS_User>().Where(conModels).ToList();
```

### 子查询加动态拼接
``` csharp
public List<Student> GetStudent(int id, string name)
{
    int pageCount = 0;
    using (var db = SugarDao.GetInstance())
    {
        //Form("Student","s")语法优化成 Form<Student>("s")
        var sable = db.Sqlable().Form<Student>("s").Join<School>("l", "s.sch_id", "l.id", JoinType.INNER);
        if (!string.IsNullOrEmpty(name))
        {
            sable = sable.Where("s.name=@name");
        }
        if (!string.IsNullOrEmpty(name))
        {
            sable = sable.Where("s.id=@id or s.id=100");
        }
        if (id > 0) {
            sable = sable.Where("l.id in (select top 10 id from school)");//where加子查询
        }
        //参数
        var pars = new { id = id, name = name };
        pageCount = sable.Count(pars);
        return sable.SelectToList<Student>("s.*", pars);
    }
}
```
