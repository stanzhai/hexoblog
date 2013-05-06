title: linux permission denied的奇怪问题
date: 2013-05-06 15:04:30
categories: [学习, 问题修复]
tags: linux
---

## 问题描述

创建了一个带有执行权限的软连接文件，放到了/usr/local/bin目录下，非root用户执行时提示权限不足。

```
[zhaishidan@molinux ~]$ ll /usr/local/bin/mongo
lrwxrwxrwx. 1 root root 19 May 6 11:59 /usr/local/bin/mongo -> /root/mongodb/mongo
[zhaishidan@molinux ~]$ /usr/local/bin/mongo 
-bash: /usr/local/bin/mongo: Permission denied
```

## 解决方案

软链接的文件权限并不代表真实文件的权限，查看/root目录权限如下：

```
dr-xr-x---.  25 root root  4096 May  6 13:43 root
```
可知其他用户没有对/root目录的访问权限，自然也就不能执行其中的程序。

解决方法是：将程序调整到`/usr/local/bin`目录下，就可以了。