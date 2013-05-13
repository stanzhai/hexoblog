title: 安装python包管理工具pip
date: 2013-04-17
categories: 学习
tags: [python, pip]
---

## 准备

pip依赖setuptools或distribute。

如果python的版本是3.X，则必须安装distribute。

<!-- more -->
因此建议安装distribute。

## 安装distribute

使用curl或者wget下载安装文件,并安装：

	wget http://python-distribute.org/distribute_setup.py
	python distribute_setup.py

安装过程中如果遇到权限不足的情况，请切换到超级管理员用户进行安装。

## 安装pip

安装完distribute就可以安装pip了。

同样使用curl或wget下载。

	$ curl -O https://raw.github.com/pypa/pip/master/contrib/get-pip.py
	$ [sudo] python get-pip.py

## 验证是否安装成功

命令行下输入`pip`如果出现Usage使用信息，说明安装成功。
