title: 使用gdb调试程序
date: 2013-04-05
categories: 学习
tags: [Linux, gdb]
---

## 前期准备

安装gcc和gdb（废话）.

使用gcc编译程序时需要指定-g参数生成调试信息。

<!-- more -->
>> gcc -g test.c -o test

## 开始调试

使用gdb test进入调试状态

** 常用参数 **

1. list: `list 50` 查看50行附近的源代码，简洁命令：l
2. break: `break 30`在30行设置断点，简洁命令：b
3. run: 启动程序开始调试，简洁命令：r
4. continue: 从断点处继续执行，简洁命令：c
5. print: `print a`查看变量a的值（当然程序得跑到断点从停下来才能查看），简洁命令p
6. info break: 查看断点信息,简洁命令：i b
7. next : 不进入单步调试
8. step：单步调试
9. call: `call printf("test")`调用并执行一个函数，使用finish结束执行。

## 补充说明

gdb这货太强大了，以上只是列举了一下常用的命令，做个学习记录，其他高级的用法，将来进行补充，看官可以自己从其他网站查找学习。
