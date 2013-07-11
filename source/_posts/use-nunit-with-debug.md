title: 如何使用NUnit的过程中进行代码调试
date: 2013-07-11 14:38:17
categories: 学习
tags: NUnit
---

## 基本的使用方法

请参考 <http://www.cnblogs.com/cnblogsfans/archive/2008/04/15/1153804.html>

<!--more-->

## 解决.NET 4.0中无法调试的问题

修改`nunit.exe.config`，将startup节点修改为：

```xml
<startup>  
  <requiredRuntime version="v4.0.30319" />
</startup>
```

参考：<http://www.cnblogs.com/GoodHelper/archive/2010/06/23/vs2010_nunit_debug.html>