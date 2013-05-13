title: Windows平台编程实现关闭显示器并锁定计算机
date: 2013-05-03 20:54:32
categories: 学习
tags: C++
---

## 为何搞个这东西？

额，最近主管给俺们这些开发的申请了双显示器，开发感觉相当爽啊。可是公司有规定，离开工位需要锁定计算机，关闭显示器。2个显示器，来回关也挺麻烦的。于是乎突然有了弄个关闭显示器玩玩的想法。
<!--more-->

另一名开发同学也有此想法，并且从网上找个了类似的程序，看了下是Delphi写的，带界面的，功能是有，不过用起来不方便。

** 我想要的是 **

有个程序一双击就可以把我的显示器关闭，同时还能锁屏。注意：这个程序可不是一直开着的哦，只要双击一下就可以了，也不麻烦。

此程序小巧，尽量不依赖其他运行环境，第一时间想到了用C或C++。

## 想想功能简单，不如自己写个，就当学习了

从网站查资料找到了2个关键功能的实现方法。

* 关闭显示器：使用`SendMessage(HWND_BROADCAST, WM_SYSCOMMAND, SC_MONITORPOWER, 2);`关于SC_MONITORPOWER消息，自己查一下MSDN吧，写的比较清楚。
* 锁定计算机：本想模拟组合键Win+L，这了些资料没有找到靠谱的方法，最后使用rundll32这个命令来实现的`ShellExecute(0,"open","rundll32.exe","USER32,LockWorkStation","",SW_SHOW);`

## 程序源代码，一个源文件搞定

```
#include <Windows.h>

int WinMain( __in HINSTANCE hInstance, 
    __in_opt HINSTANCE hPrevInstance,
    __in LPSTR lpCmdLine,
    __in int nShowCmd )
{
    // 关闭显示器
    SendMessage(HWND_BROADCAST, WM_SYSCOMMAND, SC_MONITORPOWER, 2);
    // 锁定计算机
    ShellExecute(0,"open","rundll32.exe","USER32,LockWorkStation","",SW_SHOW);
    return 0;
}
```

## 其他：给程序加图标的问题

为项目添加一个资源文件，想资源文件添加一个合适的图标，编译一下就有了。