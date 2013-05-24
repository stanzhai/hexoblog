title: 扩展C#,DateTime返回和js，Date的getTime相同的值
date: 2013-05-23 18:22:08
categories: 学习
tags: CSharp
---

最近做一个基于Web的实时图表，需要为前台提供时间信息，时间格式是类似这样的`new Date().getTime()`其实就是js实现的返回距 1970年1月1日之间的毫秒数的一个方法。

数据是由.NET做后端提供，而.NET没有可以实现和js一样的结果的方法。所以现在只好自己实现一个。我使用了扩展方法扩展了DateTime类。具体实现如下：

<!--more-->

```
public static class Helpers
{
    /// <summary>
    /// 返回和js中Date，getTime等效的值（UTC，返回距 1970 年 1 月 1 日之间的毫秒数）
    /// </summary>
    /// <param name="dt"></param>
    /// <returns></returns>
    public static long GetTime(this DateTime dt)
    {
        long lLeft = 621355968000000000;
        long tick = (dt.ToUniversalTime().Ticks - lLeft) / 10000;
        return tick;
    }
}
```

**吐槽一下**

最近被MongoDB的时间问题给搞的头大啊，默认时区为零，js中格式化日期还不方便，与.NET交互时间格式还得统一，想吐啊……