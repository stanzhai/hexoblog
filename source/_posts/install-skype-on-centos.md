title: 在CentOS 14上安装Skype
date: 2013-04-05
categories: 问题修复
tags: [Linux]
---

## 起因

Skype官方并没有直接提供针对CentOS的安装包，从官网下载rpm安装时也是提示安装错误。

本人系统是64位的，安装的Skype是32位的，需要在系统中安装32位对应的so（动态库）.

<!-- more -->
## 安装方法

** 1.添加安装源并安装 **

	$vi /etc/yum.repos.d/skype.repo

	[skype]
	name=Skype Repository
	baseurl=http://download.skype.com/linux/repos/fedora/updates/i586/
	enabled=1
	gpgkey=http://www.skype.com/products/skype/linux/rpm-public-key.asc
	gpgcheck=0

	$yum install skype

** 2.解决运行时的错误 **

安装成功后，在Applications，Internet中会出现Skype的图标，直接运行没有反映，在Terminal中运行skype提示缺少libXss.so.

从网上找到了一篇文章，根据自己缺少的包安装即可。

Primera parada:

$ skype bash: /usr/bin/skype: /lib/ld-linux.so.2: bad ELF interpreter: No existe el fichero o el directorio  
$ sudo yum install glibc.i686 ...

Segunda parada:

$ skype skype: error while loading shared libraries: libasound.so.2: cannot open shared object file: No such file or 
directory  
$ sudo yum install alsa-lib.i686 ...

Tercera parada.

$ skype skype: error while loading shared libraries: libXv.so.1: cannot open shared object file: No such file or
 directory  
$ sudo yum install libXv.i686 ...

Cuarta parada:

$ skype skype: error while loading shared libraries: libXss.so.1: cannot open shared object file: No such file or 
directory  
$ sudo yum install libXScrnSaver.i686 ...

Quinta parada:

$ skype skype: error while loading shared libraries: libQtDBus.so.4: cannot open shared object file: No such file or 
directory  
$ sudo yum install qt.i686 ...

Sexta parada:

$ skype skype: error while loading shared libraries: libQtGui.so.4: cannot open shared object file: No such file or 
directory  
$ sudo yum install qt-x11.i686 ...
