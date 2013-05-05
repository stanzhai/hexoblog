title: C#处理控制台关闭事件
date: 2013-05-05 16:49:13
categories: 学习
tags:
---

## 应用场景

我们开发的控制台应用，在运行阶段很有可能被用户Ctrl+C终止或是被用户直接关闭。如果我们不希望用户通过Ctrl+C终止我们的程序，就需要对Ctrl+C或关闭事件作处理。

<!--more-->

## 处理方法

在.net平台下Console类有个CancelKeyPress事件可以处理Ctrl+C，不过对于直接关闭控制台应用，这种处理就无能为力了。

不过Windows API中有个SetConsoleCtrlHandler函数可以处理这两种关闭事件。

C#处理代码如下：

```
static class Program
{
    public delegate bool ControlCtrlDelegate(int CtrlType);
    [DllImport("kernel32.dll")]
    private static extern bool SetConsoleCtrlHandler(ControlCtrlDelegate HandlerRoutine, bool Add);
    private static ControlCtrlDelegate cancelHandler = new ControlCtrlDelegate(HandlerRoutine);

    public static bool HandlerRoutine(int CtrlType)
    {
        switch (CtrlType)
        {
            case 0:
                Console.WriteLine("0工具被强制关闭"); //Ctrl+C关闭  
                break;
            case 2:
                Console.WriteLine("2工具被强制关闭");//按控制台关闭按钮关闭  
                break;
        }
        Console.ReadLine();
        return false;
    }  

    /// <summary>
    /// 应用程序的主入口点。
    /// </summary>
    [STAThread]
    static void Main(string[] args)
    {
        SetConsoleCtrlHandler(cancelHandler, true);
        Console.ReadLine();
    }
}
```
