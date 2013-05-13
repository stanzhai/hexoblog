title: Linux下，解决error while loading shared libraries
date: 2013-04-05
categories: 问题修复 
tags: [Linux]
---

## 问题描述

最近开发Linux应用，启动程序时提示：error while loading shared libraries: libhiredis.so.0.10: cannot open shared

hiredis是我编译好的redis客户端连接驱动，在/usr/local/lib/目录中存在libhiredis相关的so文件，推测应该是so目录缺少了/usr/local/lib这个目录。

<!-- more -->
## 解决方案

打开`/etc/ld.so.conf`这个文件，在最后一行添加:

	/usr/local/lib

最后执行：`/sbin/ldconfig -v`更新lib目录即可。
