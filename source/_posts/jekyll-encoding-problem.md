title: jekyll 中文编码问题
date: 2013-01-30
categories:  问题修复
tags: [jekyll]
---

## 问题描述

本人在Windows下，安装Ruby和jekyll。

安装完jekyll,使用jekyll命令生成静态页面时，提示：YAML Exception reading about.md: invalid byte sequence in GB2312，导致无法生成静态页面。

<!-- more -->
## 解决方案

[参考链接](https://github.com/imathis/octopress/issues/232#issuecomment-2480736)

修改：C:\Ruby193\lib\ruby\gems\1.9.1\gems\jekyll-0.11.2\lib\jekyll\convertible.rb

找到函数read_yaml，将原来的：

	self.content = File.read(File.join(base, name))

修改为：

	self.content = File.read(File.join(base, name), :encoding => "utf-8")
