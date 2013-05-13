title: MongoDB 删除document中的key
date: 2013-05-08 13:47:07
categories: 学习
tags:
---

## 需求

假设当前Document中的实体为以下形式：

```
{
  "_id" : 1234,
  "name" : "Chris",
  "description" : "Awesome"
}
```

<!--more-->

需要将其调整成以下形式：

```
{
  "_id" : 1234,
  "name" : "Chris"
}
```

## 操作方法

通过调用update方法，指定`$unset`来删除属性。

```
db.collection_name.update({ _id: 1234 }, { $unset : { description : 1} });
```
update方法默认情况下只会更新第一个匹配，如果对所有符合条件的记录做修改，则需要使用以下方法：

```
db.collection_name.update({}, { $unset : { description : 1} }, false, true);
```

其中第三个参数为upsert,这个参数是个布尔类型，默认是false。当它为true的时候，update方法会首先查找与第一个参数匹配的记录，在用第二个参数更新之，如果找不到与第一个参数匹配的的记录，就插入一条（upsert 的名字也很有趣是个混合体：update+insert）。

第4个参数设置为true表示更新所有符合条件的记录。
