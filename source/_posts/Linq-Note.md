---
title: Linq Note
date: 2021-11-01 22:05:43
tags: [Linq]
excerpt: 之前记录的关于Linq的笔记，花式Linq拓展。
categories: C#
index_img: https://gitee.com/xlzf/blog-image/raw/master/Gongsi/Csharp.jpeg
---

## 前言

近自己写个小软件，琢磨着不用之前那种拼接SQL的三层方法，DAL层想用一下Linq。总结如下：

通常都是GET，POST：

先来一波简版的：

```csharp
///查
public List<SYS_User> GetALL()
{
    return entity.SYS_Users.ToList();
}
///增
public int Add(SYS_User f)
{
    entity.SYS_Users.Add(f);

    return entity.SaveChanges();
}
///改
public int Edit(SYS_User f)
{
    entity.Entry(f).State = EntityState.Modified;

    return entity.SaveChanges();
}
///删
public int Delete(int id)
{
    SYS_User f = entity.SYS_Users.Find(id);

    entity.SYS_Users.Remove(f);

    return entity.SaveChanges();
}
```

看到这些，会想到在实际应用中，不是这样的，光说**查询**，就会遇到很多花式的查询。

然后去百度，去看大神的帖子，可能是我百度的姿势不对，找到的资料自己总结了一下。

但是还是感觉不是很舒服，可能我对`Linq`的理解太浅薄了。下面记录一下：

## 第一种方法

``` csharp
/// <summary>
/// 第一种linq封装方法查询
/// </summary>
/// <param name="keywords"></param>
/// <returns></returns>
public List<SYS_User> Get(params string[] keywords)
{
    var predicate = PredicateBuilder.True<SYS_User>();
 
    foreach (var keyword in keywords)
    {
        string temp = keyword;
 
        predicate = predicate.And(s => s.User_Name.Contains("1"));
    }
 
    //predicate = predicate.And(s => s.User_Name=="1");
    //predicate = predicate.Or(s => s.User_Name=="2");
 
    var result = entity.SYS_Users.Where(predicate).ToList();
 
    return result;
}
```

当然，对应类加个备份吧。

```csharp
/// <summary>
/// 动态查询
/// 参考URL:https://blog.csdn.net/kongwei521/article/details/27197753
/// </summary>
public static class PredicateBuilder
{
    public static Expression<Func<T, bool>> True<T>() { return f => true; }
    public static Expression<Func<T, bool>> False<T>() { return f => false; }
    public static Expression<T> Compose<T>(this Expression<T> first, Expression<T> second, Func<Expression, Expression, Expression> merge)
    {
        // build parameter map (from parameters of second to parameters of first)
        var map = first.Parameters.Select((f, i) => new { f, s = second.Parameters[i] }).ToDictionary(p => p.s, p => p.f);

        // replace parameters in the second lambda expression with parameters from the first
        var secondBody = ParameterRebinder.ReplaceParameters(map, second.Body);

        // apply composition of lambda expression bodies to parameters from the first expression 
        return Expression.Lambda<T>(merge(first.Body, secondBody), first.Parameters);
    }

    public static Expression<Func<T, bool>> And<T>(this Expression<Func<T, bool>> first, Expression<Func<T, bool>> second)
    {
        return first.Compose(second, Expression.And);
    }

    public static Expression<Func<T, bool>> Or<T>(this Expression<Func<T, bool>> first, Expression<Func<T, bool>> second)
    {
        return first.Compose(second, Expression.Or);
    }

}

public class ParameterRebinder : ExpressionVisitor
{
    private readonly Dictionary<ParameterExpression, ParameterExpression> map;
    public ParameterRebinder(Dictionary<ParameterExpression, ParameterExpression> map)
    {
        this.map = map ?? new Dictionary<ParameterExpression, ParameterExpression>();
    }
    public static Expression ReplaceParameters(Dictionary<ParameterExpression, ParameterExpression> map, Expression exp)
    {
        return new ParameterRebinder(map).Visit(exp);
    }
    protected override Expression VisitParameter(ParameterExpression p)
    {
        ParameterExpression replacement;
        if (map.TryGetValue(p, out replacement))
        {
            p = replacement;
        }
        return base.VisitParameter(p);
    }
}
```

![img](https://gitee.com/xlzf/blog-image/raw/master/521993-20181005160328572-1728196450.png)

## 第二种方法

``` csharp
/// <summary>
/// 第二种linq封装方法动态查询
/// </summary>
public void GetDong()
{
    #region 试水

    var query = SpecificationBuilder.Create<SYS_User>();

    query = query.Equals(d => d.User_Name, "1");

    query = query.Equals(d => d.Phone, "1");

    var result = entity.SYS_Users.Where(query.Predicate).ToList();

    #endregion

   #region 执行Json动态查询
    //{a => (((a.User_Name == "1") And (a.Phone == "1")) Or (a.Phone == "ccccc"))}
    string queryJson = "{\"[EQUAL][And]User_Name\":\"1\",\"[EQUAL][And]Phone\":\"1,ccccc\"}";

    if (!string.IsNullOrEmpty(queryJson))
    {
        var predicate = Utils.GetSerchExtensions<SYS_User>(queryJson);

        var result1 = entity.SYS_Users.Where(predicate).ToList();

        //query.Predicate = query.Predicate.And(predicate);
    }

    #endregion

    #region 组合单个查询条件

    QueryEntity queryEntity = new QueryEntity()
    {
        LogicOperation = LogicOperation.EQUAL,
        PredicateType = PredicateType.AND,
        Column = "User_Name",
        Value = "1"
    };

    var qqqqq = Utils.GetSerchExtensions<SYS_User>(new List<QueryEntity>() { queryEntity });

    var a = entity.SYS_Users.Where(qqqqq).ToList();

    #endregion

    #region 组合多个查询条件

    List<QueryEntity> queryEntities = new List<QueryEntity>();

    queryEntities.Add(new QueryEntity
                      {
                          LogicOperation = LogicOperation.EQUAL,
                          PredicateType = PredicateType.AND,
                          Column = "User_Name",
                          Value = "1"
                      });

    queryEntities.Add(new QueryEntity
                      {
                          LogicOperation = LogicOperation.EQUAL,
                          PredicateType = PredicateType.AND,
                          Column = "Phone",
                          Value = "1"
                      });

    qqqqq = Utils.GetSerchExtensions<SYS_User>(queryEntities);

    a = entity.SYS_Users.Where(qqqqq).ToList();

    #endregion
}
```

 缺的方法在这：

```csharp
/// <summary>
/// 手动去拼接动态表达式
/// </summary>
/// <typeparam name="T"></typeparam>
public class ExpressionHandle<T> where T : class
{
    public Expression PrepareConditionLambda(IList<ParamObject> paramList, ParameterExpression paramExp)
    {
        List<Expression> expList;
        if (null == paramList)
        {
            expList = new List<Expression>(1);
            var valueEqual = Expression.Constant(1);
            var expEqual = Expression.Equal(valueEqual, valueEqual);
            expList.Add(expEqual);
        }
        else
        {
            expList = new List<Expression>(paramList.Count);
            #region 筛选条件

                foreach (var p in paramList)
                {
                    var exp = Expression.Property(paramExp, p.Name);
                    var propertyType = typeof(T).GetProperty(p.Name).PropertyType; //得到此字段的数据类型
                    var value = propertyType == typeof(Guid?) ? new Guid(p.Value.ToString()) : Convert.ChangeType(p.Value, TypeHelper.GetUnNullableType(propertyType));
                    switch (p.Operation)
                    {
                        case LogicOperation.LIKE:
                            var containsMethod = typeof(string).GetMethod("Contains");
                            var valueLike = Expression.Constant(value, propertyType);
                            var expLike = Expression.Call(exp, containsMethod, valueLike);
                            expList.Add(expLike);
                            break;
                        case LogicOperation.EQUAL:
                            var valueEqual = Expression.Constant(value, propertyType); //值
                            var expEqual = Expression.Equal(exp, valueEqual); //拼接成 t=>t.name=valueEqual
                            expList.Add(expEqual);
                            break;
                        case LogicOperation.LT:
                            var valueLT = Expression.Constant(value, propertyType); //值
                            var expLT = Expression.LessThan(exp, valueLT); //拼接成 t=>t.name<valueEqual
                            expList.Add(expLT);
                            break;
                        case LogicOperation.GT:
                            var valueGT = Expression.Constant(value, propertyType);
                            var expGT = Expression.GreaterThan(exp, valueGT);
                            expList.Add(expGT);
                            break;
                        case LogicOperation.NOTEQUAL:
                            var valuent = Expression.Constant(value, propertyType);
                            var expnt = Expression.NotEqual(exp, valuent);
                            expList.Add(expnt);
                            break;
                            //case LogicOperation.CONTAINS:
                            //     //var valueCT = Expression.Constant(value, propertyType);
                            //    var type = typeof(string);
                            //    var expCT = Expression.Constant(exp, type);
                            //     expList.Add(expCT);
                            //    break;
                    }
                }
            #endregion
        }

        return expList.Aggregate<Expression, Expression>(null, (current, item) => current == null ? item : Expression.And(current, item));
    }
}

/// <summary>
/// 
/// </summary>
public class QueryModel
{
    IList<ParamObject> _items = new List<ParamObject>();
    public IList<ParamObject> Items
    {
        get
        {
            return _items;
        }
        set
        {
            _items = value;
        }
    }
}

/// <summary>
/// 参数
/// </summary>
public class ParamObject
{
    public string Name { get; set; }
    public LogicOperation Operation { get; set; }
    public object Value { get; set; }
}

/// <summary>
/// 排序
/// </summary>
public class SortObject
{
    public string OrderBy { get; set; }
    public SortOperation Sort { get; set; }
}

/// <summary>
/// 排序方式
/// </summary>
public enum SortOperation
{
    ASC,
    DESC
}

/// <summary>
/// 查询方式
/// </summary>
public enum LogicOperation
{
    LIKE,    //包含,模糊查询
    EQUAL,   //等于
    LT,      //小于
    GT,       //大于
    CONTAINS,       //包含，In查询 
    NOTEQUAL       //不等于 
}

/// <summary>
/// 
/// </summary>
public static class TypeHelper
{
    public static Type GetUnNullableType(Type conversionType)
    {
        if (conversionType.IsGenericType && conversionType.GetGenericTypeDefinition() == typeof(Nullable<>))
        {
            //如果是泛型方法，且泛型类型为Nullable<>则视为可空类型
            //并使用NullableConverter转换器进行转换
            var nullableConverter = new System.ComponentModel.NullableConverter(conversionType);
            conversionType = nullableConverter.UnderlyingType;
        }
        return conversionType;
    }
}

/// <summary>
/// 连接方式
/// </summary>
public enum PredicateType
{
    AND, OR
}

/// <summary>
/// 查询方式
/// </summary>
public enum SearchType
{
    Between, Like, Equals
}

/// <summary>
/// 查询实体
/// </summary>
public class QueryEntity
{
    /// <summary>
    /// 查询方式
    /// </summary>
    public LogicOperation LogicOperation { get; set; }

    /// <summary>
    /// 连接方式
    /// </summary>
    public PredicateType PredicateType { get; set; }

    /// <summary>
    /// 列名
    /// </summary>
    public string Column { get; set; }

    /// <summary>
    /// 列值
    /// </summary>
    public object Value { get; set; }
}
}
```

```csharp
public class Utils
{
    #region 把查询条件拼接为Extensions
        /// <summary>
        /// 把查询条件拼接为Extensions
        /// </summary>
        /// <typeparam name="TEntity">查询实体</typeparam>
        /// <param name="searchJson">查询条件，例如：[like][or]name:123</param>
        /// <returns></returns>
        public static Expression<Func<TEntity, bool>> GetSerchExtensions<TEntity>(String searchJson) where TEntity : class, new()
    {
        try
        {
            var ja = (JArray)JsonConvert.DeserializeObject("[" + searchJson + "]");                     //把查询条件转换为Json格式
            var enumerableQuery = new EnumerableQuery<KeyValuePair<string, JToken>>(ja[0] as JObject);
            return GetSerchExtensions<TEntity>(enumerableQuery);    //把查询条件拼接为Extensions
        }
        catch (Exception)
        {
            throw;
        }
    }

    /// <summary>
    /// 把查询条件拼接为Extensions
    /// </summary>
    /// <typeparam name="TEntity">查询实体</typeparam>
    /// <param name="enumerableQuery">查询条件</param>
    /// <returns></returns>
    public static Expression<Func<TEntity, bool>> GetSerchExtensions<TEntity>(EnumerableQuery<KeyValuePair<string, JToken>> enumerableQuery) where TEntity : class, new()
    {
        var paramExp = Expression.Parameter(typeof(TEntity), "a");
        if (null == enumerableQuery || !enumerableQuery.Any())
        {
            var valueEqual = Expression.Constant(1);
            var expEqual = Expression.Equal(valueEqual, valueEqual);
            return Expression.Lambda<Func<TEntity, bool>>(expEqual, paramExp);  //如果参数为空，返回一个a=>1=1 的值

        }
        var modeltypt = typeof(TEntity);  //实体类型
        var keyList = enumerableQuery.Select(e => e.Key).ToList();      //取出Json 的每个字符串

        Expression whereExp = null;
        keyList.ForEach(s =>
                        {
                            var searchTypeStr = s.Substring(1, s.LastIndexOf("][", StringComparison.Ordinal) - 1);   //查询方式   Like
                            var ab = s.Substring(s.LastIndexOf("][", StringComparison.Ordinal) + 2);
                            var joinTypeStr = ab.Remove(ab.LastIndexOf("]", StringComparison.Ordinal));              //连接方式   or
                            var searchField = s.Substring(s.LastIndexOf("]", StringComparison.Ordinal) + 1);         //查询的列名 name 
                            var value = enumerableQuery.FirstOrDefault(v => v.Key == s).Value.ToString();            //值         123

                            LogicOperation searchType;       //查询方式
                            PredicateType joinType;           //连接方式

                            if (Enum.TryParse(searchTypeStr.ToUpper(), out searchType) && Enum.TryParse(joinTypeStr.ToUpper(), out joinType) && modeltypt.GetProperties().Any(p => String.Equals(p.Name, searchField, StringComparison.CurrentCultureIgnoreCase)))  //这个实体有这个列名
                            {
                                var firstOrDefault = modeltypt.GetProperties().FirstOrDefault(p => String.Equals(p.Name, searchField, StringComparison.CurrentCultureIgnoreCase));
                                if (firstOrDefault == null) return;
                                var selCol = firstOrDefault.Name;  //查询的列名
                                var splitList = value.Split(',').ToList();   //这个位置是的处理是默认认为当查询值中包含,的视为或者的查询：例如 A='abc,def' 处理成 (A='def' OR  A='abc'),但是时间上这块无法满足就要查询包含,的数据的求
                                for (var i = 0; i < splitList.Count; i++)
                                {
                                    var val = splitList[i];

                                    if (val == null || string.IsNullOrWhiteSpace(val)) continue;
                                    var expressionFuncEquals = PrepareConditionLambda<TEntity>(selCol, val, paramExp, searchType); //得到这个查询的表达式
                                    whereExp = i != 0 ? (whereExp == null ? expressionFuncEquals : Expression.Or(whereExp, expressionFuncEquals)) : (joinType == PredicateType.OR ? (whereExp == null ? expressionFuncEquals : Expression.Or(whereExp, expressionFuncEquals))
                                                                                                                                                     : (whereExp == null ? expressionFuncEquals : Expression.And(whereExp, expressionFuncEquals)));
                                }
                            }
                        });
        return Expression.Lambda<Func<TEntity, bool>>(whereExp, paramExp); ;
    }

    /// <summary>
    /// 把查询条件拼接为Extensions
    /// </summary>
    /// <typeparam name="TEntity">实体类</typeparam>
    /// <param name="queryEntitys">查询实体</param>
    /// <returns></returns>
    public static Expression<Func<TEntity, bool>> GetSerchExtensions<TEntity>(List<QueryEntity> queryEntitys) where TEntity : class, new()
    {
        var paramExp = Expression.Parameter(typeof(TEntity), "a");
        if (null == queryEntitys || !queryEntitys.Any())
        {
            var valueEqual = Expression.Constant(1);
            var expEqual = Expression.Equal(valueEqual, valueEqual);
            return Expression.Lambda<Func<TEntity, bool>>(expEqual, paramExp);  //如果参数为空，返回一个a=>1=1 的值

        }
        var modeltypt = typeof(TEntity);  //实体类型
        Expression whereExp = null;

        queryEntitys.ForEach(q =>
                             {
                                 LogicOperation searchType = q.LogicOperation;       //查询方式
                                 PredicateType joinType = q.PredicateType;           //连接方式
                                 var searchField = q.Column;         //查询的列名 name 
                                 var value = q.Value;            //值         123
                                 if (modeltypt.GetProperties().Any(p => String.Equals(p.Name, searchField, StringComparison.CurrentCultureIgnoreCase)))  //这个实体有这个列名
                                 {
                                     var firstOrDefault = modeltypt.GetProperties().FirstOrDefault(p => String.Equals(p.Name, searchField, StringComparison.CurrentCultureIgnoreCase));
                                     if (firstOrDefault == null) return;
                                     var selCol = firstOrDefault.Name;  //查询的列名
                                     var splitList = value.ToString().Split(',').ToList();   //这个位置是的处理是默认认为当查询值中包含,的视为或者的查询：例如 A='abc,def' 处理成 (A='def' OR  A='abc'),但是时间上这块无法满足就要查询包含,的数据的求
                                     for (var i = 0; i < splitList.Count; i++)
                                     {
                                         if (splitList[i] == null || string.IsNullOrWhiteSpace(splitList[i])) continue;
                                         var expressionFuncEquals = PrepareConditionLambda<TEntity>(selCol, splitList[i], paramExp, searchType); //得到这个查询的表达式
                                         whereExp = i != 0
                                             ? (whereExp == null ? expressionFuncEquals : Expression.Or(whereExp, expressionFuncEquals))
                                             : (joinType == PredicateType.OR ? (whereExp == null ? expressionFuncEquals : Expression.Or(whereExp, expressionFuncEquals))
                                                : (whereExp == null ? expressionFuncEquals : Expression.And(whereExp, expressionFuncEquals)));
                                     }
                                 }
                             });
        return Expression.Lambda<Func<TEntity, bool>>(whereExp, paramExp); 
    }


    /// <summary>
    /// 得到字段查询的表达式
    /// </summary>
    /// <typeparam name="TEntity">实体</typeparam>
    /// <param name="name">查询列名</param>
    /// <param name="dateValue">数据值</param>
    /// <param name="paramExp">参数</param>
    /// <param name="searchType">查询方式(默认是等于查询)</param>
    /// <returns></returns>
    private static Expression PrepareConditionLambda<TEntity>(string name, object dateValue, ParameterExpression paramExp, LogicOperation searchType = LogicOperation.EQUAL)
    {
        if (dateValue == null) throw new ArgumentNullException("dateValue");
        if (paramExp == null) throw new ArgumentNullException("paramExp");
        var exp = Expression.Property(paramExp, name);
        var propertyType = typeof(TEntity).GetProperty(name).PropertyType; //得到此字段的数据类型
        object value; //propertyType == typeof(Guid?) ? new Guid(dateValue.ToString()) : Convert.ChangeType(dateValue, TypeHelper.GetUnNullableType(propertyType));
        if (propertyType == typeof(Guid?) || propertyType == typeof(Guid))
        {
            value = new Guid(dateValue.ToString());
        }
        else
        {
            value = Convert.ChangeType(dateValue, TypeHelper.GetUnNullableType(propertyType));
        }
        Expression expEqual = null;
        switch (searchType)
        {
            case LogicOperation.EQUAL:   //等于查询
                var valueEqual = Expression.Constant(value, propertyType); //值
                expEqual = Expression.Equal(exp, valueEqual); //拼接成 t=>t.name=valueEqual
                break;
            case LogicOperation.LIKE:   //模糊查询
                var containsMethod = typeof(string).GetMethod("Contains");
                var valueLike = Expression.Constant(value, propertyType);
                expEqual = Expression.Call(exp, containsMethod, valueLike);
                break;
        }
        return expEqual;
    }
    #endregion
}
```

![img](https://gitee.com/xlzf/blog-image/raw/master/521993-20181005154806206-690330382.png)

![img](https://gitee.com/xlzf/blog-image/raw/master/521993-20181005154841037-1651076712.png)

![img](https://gitee.com/xlzf/blog-image/raw/master/521993-20181006205821996-188650883.png)
