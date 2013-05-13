title: 调用线程必须为 STA，因为许多 UI 组件都需要
date: 2013-04-16
categories: 问题修复
tags: [WPF, 多线程]
---

## 问题描述

最近用WPF开发桌面端应用，用到了个组件，此组件内部使用了多线程，在处理组件产生的事件时，需要和UI交互。

处理UI交互的代码抛出了如下异常：调用线程必须为 STA，因为许多 UI 组件都需要。

<!-- more -->
## 异常描述

WPF的UI线程属于单一线程单元模型的，多线程环境下，必须为线程指定STA属性才能与UI交互。

** 关于STA **

STA不是单线程的意思.英文为single threaded apartment.是一种套间(或译为公寓)线程模式.

sta thread并不表明应用程式的类型,和应用程序不搭界,恰相反,一个应用程序可以有多个线程.每个线程也可以有多个组件或对象.

以前win16位系统的组件线程模式才真正是单线程.这是一种被淘汰了的模式.
线程模式用于处理组件在多线程的环境里并行与并互的方式.比如套间线程(STAThread)模式中接口跨线程传递必须被调度(Marshal),不调度直传肯定会失败!而MTA或FreeThread模式中的接口可以不经调度直接传递.

## 解决方案

在处理UI交互的地方，创建单独的线程，并为线程指定STA特性，在与UI交互就ＯＫ了。

示例：

    private void Handler(string s, byte[] bytes)
    {
        string data = Encoding.UTF8.GetString(bytes);
        // 由于UI线程必须在单一线程单元模型中运行，故单独开辟线程来运行UI
        ParameterizedThreadStart threadStart = ThreadStart;
        Thread thread = new Thread(threadStart);
        thread.SetApartmentState(ApartmentState.STA);
        thread.IsBackground = true;
        thread.Start(data);
    }

    private void ThreadStart(object o)
    {
        string data = o as string;
        AlertWindow alertWindow = new AlertWindow(data);
        alertWindow.Show();
        System.Windows.Threading.Dispatcher.Run();
    }

线程函数中需要指定`System.Windows.Threading.Dispatcher.Run();`我们在此线程的控制下创建一个新窗口。WPF 自动创建一个新的 Dispatcher 来管理新线程。要使该窗口工作，只需启动 Dispatcher。
