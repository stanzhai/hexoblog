title: 使用js替换字符串
date: 2013-04-08
categories: 学习
tags: [js]
---


## 使用js替换字符需要注意的问题

js中使用replace方法替换字符串是，如果使用这种方式：`str.replace('a', 'b')`那么str中只有第一个a会被替换成b。

还需要注意的是replace方法不会修改str中的原始内容，它会返回替换后的结果。
<!-- more -->

## 如何替换所有匹配信息

使用正则表达式替换。

如：`str.replace(/a/g, 'b')`会将str中所有的a替换为b。
