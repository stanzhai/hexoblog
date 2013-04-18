title: Eclipse:Failed to load the JNI shared library
date: 2013-03-17
categories: 问题修复
tags: [Eclipse]
---


## 问题描述

启动Eclipse出现如下错误：

![Eclipse error](/images/eclipse_error.png)
<!-- more -->

## 解决方案

启动`cmd`控制台，查看Java版本:`java -version`，版本信息中可以查看是否为32位，如果是32位请使用32位的Eclipse，如果是64位请使用64位的Eclipse即可。
