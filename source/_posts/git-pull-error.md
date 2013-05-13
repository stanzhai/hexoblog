title: 解决git pull更新版本库失败的问题
date: 2013-04-05
categories: 问题修复
tags: [Linux, git]
---

## 错误描述

今天使用git pull来更新版本库，提示如下错误：

	git error: unable to resolve reference refs/remotes/origin/master:

<!-- more -->
## 解决方案

参考http://stackoverflow.com/questions/2998832/git-pull-fails-unalble-to-resolve-reference-unable-to-update-local-ref 中提供的解决方法。

我使用

	rm .git/refs/remotes/origin/master
	git fetch

修复了此错误。

## 附，今天(2013-4-8)又遇到一个问题

使用`git push`提交代码，遇到403 Forbidden的错误,在网上查了下，原来是git版本过低的问题。

使用`yum remove git`,删除原来使用yum安装的git，从github下载源代码，参考INSTALL文件中的安装说明，安装之。

