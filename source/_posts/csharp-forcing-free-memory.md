title: C#强制释放内存
date: 2013-07-01 15:31:41
categories: 学习
tags: CSharp
---

最近对部门的爬虫做内存优化，起初重写了核心的爬取组件，内存占用确实低了很多，不过代码中有大量的文本处理，加之.net对string类型的特殊处理，爬虫长时间运行后，内存占用还是很高，使用VMMap分析后，发现Working Set中Managed Heap（托管堆）占用比例最大。

<!--more-->

## 使用EmptyWorkingSet进行强制内存释放

尽管代码中也是用GC.Collect进行手动垃圾回收，但是效果始终不明显。

查资料的时候无意发现了EmptyWorkingSet这个WindowsAPI，可以尽可能多的从Working Set中回收内存，不少内存管理优化的软件也用到了这个API。

**C#中的使用方法**

```
// 声明
[DllImport("psapi.dll")]
public static extern bool EmptyWorkingSet(IntPtr hProcess);

// 使用
// 获取当前进程句柄
Process pProcess = Process.GetCurrentProcess();

// 尽可能多的清空Working Set释放内存
bool bRes = EmptyWorkingSet(pProcess.Handle);
if (!bRes)
{
  // empty working set fail
}
```



## 附：Windows中的进程的Working Set，Private Bytes和Virtual Bytes

1. Working Set看成一个进程可以用到(但不一定会使用)的物理内存。即不引起page fault异常就能够访问的内存。
     Working Set包含了可能被其他程序共享的内存, 例如DLL就是一个典型的可能被其他程序共享的资源。
     所以所有进程的Working Set加起来有可能大于实际的物理内存。
2. Private Bytes是只被本进程用占用的虚拟地址空间，不包括其他进程共享的内存。
     Private Bytes既包括不引起page fault异常就能够访问的内存也包括引起page fault异常才能够访问的内存。
     所以一般Private Bytes大于Working Set。但是如果一个进程和其他进程共享较多内存，也可能造成Working Set大于Private Bytes。
3. Virtual Byte是整个进程占用的全部虚拟地址空间。32位Windows用户模式下，进程最大可以使用2GB，可以通过修改Boot.ini文件扩展为最大可以使用到3GB。
4. Windows Task Manager中看到内存使用量是Working Set。