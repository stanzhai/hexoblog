title: C#处理未捕获的异常
date: 2013-06-17 14:25:30
categories: 学习
tags:
---

尽管在程序中添加了try...catch来处理异常，也很难保证程序就不抛异常，程序崩溃是件令人很不爽的事情，在.net应用程序级别，如何体面的处理异常呢？

<!--more-->

核心代码如下：

```
// 此事件处理程序用于捕获UI线程中未处理的异常
Application.ThreadException += new ThreadExceptionEventHandler(ErrorHandlerForm.Form1_UIThreadException);

// 强制将所有UI线程中的异常交由Application.ThreadException处理
Application.SetUnhandledExceptionMode(UnhandledExceptionMode.CatchException);

// 处理非UI线程中未处理的异常
AppDomain.CurrentDomain.UnhandledException +=
    new UnhandledExceptionEventHandler(CurrentDomain_UnhandledException);

```

程序中有些异常出现后，必须要关闭程序，以免影响应用程序状态，我们可以再处理异常的代码中记录日志，保存应用程序状态信息，妥善的处理异常，给用户友好的体验。