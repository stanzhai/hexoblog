title: Linq中使用ExecuteCommand注意事项
date: 2013-03-27
categories: 问题修复
tags: [Linq]
---

## 注意事项

1. 表名不能直接作为参数传递
2. command参数中的字符类型不必加引号

这主要是因为ExecuteCommand内部会自动回参数类型做封装。
<!-- more -->

## 一个正确用法的使用示例

	dataContext.ExecuteCommand("update " + UrlMap.MapToTable(url) + " set tag={0} where url={1}",
        type,
        url);

上述语句中Url.MapToTable表示根据url判断对应的表名。

url是string类型的，command中不必加引号。
