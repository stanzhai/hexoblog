title: C#在泛型类中，通过表达式树构造lambda表达式
date: 2013-05-06 22:18:01
categories: 学习
tags: 
---

## 场景

最近对爬虫的数据库架构做调整，需要将数据迁移到MongoDB上去，需要重新实现一个针对MongoDB的Dao泛型类，好吧，动手开工，当实现删除操作的时候问题来了。

<!--more-->

我们的删除操作定义如下：`void Delete(TEntity entity)`。TEntity是我们的泛型类。

而MongoDB官方驱动自带的删除操作是这样的：

```
// 假设数据模型为已定义的Article
var query = Query<Article>.EQ(t => t.Id, id);
coll.Remove(query);
```

Dao操作的接口是不能修改的，这就要求我们必须实现以下操作：

1. 获取entity的Id值
2. 构造lambda表达式用于获取Id属性

## 实现

对于第1个好办，直接通过反射拿就可以了，至于第2个构造lambda表达式却不知该如何下手了。

在网上查资料了解到`C# Lambda表达式树允许我们像处理数据（比如读取，修改）一样来处理Lambda表达式。`。这就有方向了，研究了一下表达式树的相关知识，历经坎坷终于将其实现。

我用到的lambda表达式比较简单，也容易构造，代码中看注释应该就明白了，代码：

```
/// <summary>
/// 因为使用的Mongodb，每个数据模型必定包含Id属性，通过Id属性来删除实体
/// </summary>
/// <param name="entity"></param>
public void Delete(TEntity entity)
{
    var coll = _db.GetCollection<TEntity>(typeof(TEntity).Name);
    if (entity == null)
    {
        return;
    }
    ObjectId id = (ObjectId)typeof(TEntity).GetProperty("Id").GetValue(entity, null);

    // 通过表达式树构造lambda表达式{t => t.Id}
    // 构造调用目标t
    var target = Expression.Parameter(typeof(TEntity), "t");
    // 构造对t的属性Id的表达式
    MemberExpression bodyExp = Expression.Property(
        target,
        "Id");

    // 构造完整的lambda表达式
    Expression<Func<TEntity, ObjectId>> selector =
        Expression.Lambda<Func<TEntity, ObjectId>>(bodyExp, new [] { target });

    // 使用泛型前的语句： Query<Article>.EQ(t => t.Id, id);
    var query = Query<TEntity>.EQ(selector, id);
    coll.Remove(query);
}
```

## 参考资料

1. [C# Lambda表达式树浅谈](http://www.csharpwin.com/csharpspace/5379r5603.shtml)
2. [C#教程:lambda表达式转换成表达式树](http://www.webjx.com/aspnet/2009-04-12/11231.html)

## 用到的工具

之前都是直接使用lambda表示，而且用的还很Happy，今天遇到的问题，让我很傻眼，基础还得巩固啊。今天也是第一次调试lambda表达式，用到了[Expression Tree Visualizer for VS 2010](http://exprtreevisualizer.codeplex.com/releases?ReleaseId=88943)这个小工具。在项目调试过程中可以比较直观的查看编译好的lambda表达式。

安装和使用方法，请参见:[在Visual Studio 2010设置Expression Tree Visualizer](http://zhangyue.info/?p=351)