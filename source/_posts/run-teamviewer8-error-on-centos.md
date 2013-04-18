title: 解决在CentOS中运行TeamViewer8报错的问题
date: 2013-04-05
categories: 问题修复
tags: [Linux, TeamViewer]
---

## 错误信息

![](/images/teamviewer_error.png)

## 解决方案

<!-- more -->
从网上搜了下这个错误的解决方法，都是针对Windows系统的，无奈，我给TeamViewer的技术支持发了封邮件，寻找解决方法。

官方回复如下：

	In the Home directory, please navigate to the /.config folder and delete the TeamViewer 8 directory, you may also rename it if you are not comfortable in deleting this folder entirely. This will effectively delete the TeamViewer 8 WINE profile.

	When you relaunch TeamViewer 8, it will rebuild this directory and you should no longer get this message.

大体意思，进入用户主目录，删除.config/目录下的TeamViewer相关的文件夹，Wine会重新创建配置。

按照官方提供的解决方法，执行后，重新启动TeamViewer果真可以了，哈哈！
