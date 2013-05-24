title: MongoDB中日期的时区问题
date: 2013-05-09 16:38:20
categories: 问题修复
tags: MongoDB
---

## 问题描述

使用MongoDB，发现插入到数据库中的时间总是晚了8个小时，如：`"Insert" : ISODate("2013-05-13T01:32:44.401Z")`，突然想到，我们所在的时区为东八区，难道MongoDB中默认时区为0，到网上查了一下还真是这样`MongoDB采用标准时区存储日期`。

## 解决方案

这个问题需要在程序中作处理，本人使用的C#版的官方MongoDB驱动，在数据模型上添加BsonDateTimeOptions特性即可，如：

```
[BsonDateTimeOptions(Kind = DateTimeKind.Local)]
public DateTime Publish { get; set; }
```