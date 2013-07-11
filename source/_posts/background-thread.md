title: C#后台线程与前台线程的区别
date: 2013-07-09 23:54:44
categories: 学习
tags: CSharp
---

C#线程分为前台线程和后台线程，后台线程与前台线程类似，区别是后台线程不会防止进程终止。当所有的前台线程都结束时，clr会将应用程序进程结束，所有剩余的后台线程都会停止且不会完成。

<!--more-->

通过线程对象的`IsBackground`属性来设置是否为后台线程。