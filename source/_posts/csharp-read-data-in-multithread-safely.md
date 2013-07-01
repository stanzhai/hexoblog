title: C#在多线程环境中，进行安全遍历操作
date: 2013-06-27 11:41:05
categories: 学习
tags: CSharp
---

## 本文以List作为操作对象

**MSDN官方给出的List的线程安全的说法：**

此类型的公共静态成员是线程安全的。但不能保证任何实例成员是线程安全的。

只要不修改该集合，List 就可以同时支持多个阅读器。通过集合枚举在本质上不是一个线程安全的过程。在枚举与一个或多个写访问竞争的罕见情况下，确保线程安全的唯一方法是在整个枚举期间锁定集合。若要允许多个线程访问集合以进行读写操作，则必须实现自己的同步。

<!--more-->

## 如果不进行同步操作？

假如一个线程进行删除操作，一个线程进行遍历操作，那么在遍历过程中，集合被修改，会导致出现InvalidOperationException的异常，提示：集合已修改；可能无法执行枚举操作。

## 如何同步，保证遍历的安全

这里使用了临界区，互斥锁来保证线程遍历过程的安全，示例代码如下：

```c#
using System;
using System.Collections.Generic;
using System.Threading;

namespace ConsoleApp
{
    class Program
    {
        public static List<string> simpleList = new List<string>();
        
        public static void Main(string[] args)
        {
            // 临界区对象
            object lockObj = new object();

            // 向List中添加测试数据
            string[] data = { "1", "2", "3", "4", "5", "6", "7", "8" };
            simpleList.AddRange(data);
            // 此线程用于遍历数组
            new Thread(new ThreadStart(() =>
            {
                // 用于同步，进入临界区，只有遍历完，释放临界区对象的互斥锁，才能进行写操作
                lock (lockObj)
                {
                    foreach (var item in simpleList)
                    {
                        Console.WriteLine(item);
                        Thread.Sleep(500);
                    }
                }
            })).Start();
            // 此线程执行删除操作
            new Thread(new ThreadStart(() =>
            {
                lock (lockObj)
                {
                    simpleList.RemoveAt(0);
                    Console.WriteLine("rm 1");
                    Thread.Sleep(500);
                    simpleList.RemoveAt(0);
                    Console.WriteLine("rm 2");
                }
            })).Start();

            Console.ReadLine();
        }
    }
}
```