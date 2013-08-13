title: C#在多线程环境中使用SynchronizationContext更新控件状态
date: 2013-08-13 19:42:21
categories: 学习
tags: CSharp
---

C# 不允许UI线程之外的线程直接操作UI。使用SynchronizationContext实现跨线程更新UI是微软官方推荐的做法。

<!--more-->

一个简单的例子如下：

```
// 定义
private SynchronizationContext _syncContext;
// 实例化
_syncContext = SynchronizationContext.Current;
// 在线程中使用如下代码完成跨线程更新
_syncContext.Post(UpdateSpiderStatus, null);
// 其中UpdateSpiderStatus函数中实现了具体更新的UI的方法
```

具体使用请参考：<http://msdn.microsoft.com/zh-cn/library/system.threading.synchronizationcontext.aspx>