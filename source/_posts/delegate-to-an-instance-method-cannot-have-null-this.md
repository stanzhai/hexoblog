title: 使用Linq过程中遇到：实例方法的委托不能具有空“this”
date: 2013-04-12
categories: 问题修复
tags: []
---

## 问题描述

今天使用Linq的Any方法遇到了：实例方法的委托不能具有空“this”异常。

代码如下：

<!-- more -->
	keywords.Any(info.Title.Contains)

Any需要的参数是Func类型的，也就是个委托。异常信息是说，在调用委托方法时，没有实例，也就是说Contains是实例方法，但Title为null。

## 解决方法

根据以上分析就好解决错误了：

将代码调整为：

	info.Title != null && keywords.Any(info.Title.Contains)

这样运行就不会抛异常了。

## 感言

哎，找了老半天，误以为是多线程导致的，其实不然啊。
