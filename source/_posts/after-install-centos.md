title: 安装完CentOS后需要做的几件事
date: 2013-04-14
categories: 学习
tags: [Linux]
---

## 安装第三方YUM软件源

yum 是 centos 下很方便的 rpm 包管理工具,配置第三方软件库使你的软件库更加丰富。

建议添加EPEL，RPMForge, RPMFusion。

<!-- more -->
添加完RPMFusion后就可以安装音频视频相关的解码包了。

## 中文字体问题

1. 到xp或者vista下复制字体  
雅黑：msyh  
黑体：SimHei  
宋体：SimSun  
华文细黑：STXihei  
华文楷体：STKaiti  
等等 你要的字体

2、将要的字体复制到/usr/share/fonts/chinese/TrueType目录下

3、修改字体权限，使root以外的用户可以使用这些字体。

4、建立字体缓存，命令：cd /usr/share/fonts/chinses/TrueType

	mkfontscale
	mkfontdir
	fc-cache -fv

5、重启生效

## 玩转桌面

Compiz3D桌面效果

	yum install compiz*
	yum install fusion-icon*

仿Mac托盘图标

Cairo-Dock

