title: Win2003,解决远程桌面“终端服务器超出了最大允许连接数”
date: 2013-05-07 17:57:03
categories: 问题修复
tags:
---

## 问题描述

Win2003,远程登陆退出的时候没有点击“注销”，导致有多个用户一直留在系统中，而2个用户就是win2003的“最大允许连接数”，再次登录就会遇到这个问题。

<!--more-->

## 解决方案

已管理服务器的身份连接到远程机器。

开始->运行->cmd->mstsc /admin /v 远程机器的ip->回车。

可以通过mstsc /?查看各个参数的作用。

参数说明如下：

![](/images/mstsc.png)



