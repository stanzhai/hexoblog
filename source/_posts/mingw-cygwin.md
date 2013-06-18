title: MinGW和Cygwin的区别
date: 2013-06-08 13:48:56
categories: 学习
tags:
---

Unix下编译通过的C代码,在win32下编译是不能通过的 ,当然Unix 和win32的API都是符合标准C,也就是说,大多数函数调用在unix和win32下是相同的.但是,unix有自己一些独特的API（如fork,spawn,signals,select,sockets等）,如果代码中使用了这些API,在win32下当然找不到对应的库.

<!--more-->

但是,这些API的功能在win32中也能实现,也许你已经发现了一个能让window编译Unix风格代码的方法.

## MinGW的做法

修改编译器,让window下的编译器把诸如fork的调用翻译成等价的形式--这就是mingw的做法.

## Cygwin的做法

修改库,让window提供一个类似unix提供的库,他们对程序的接口如同unix一样,而这些库,当然是由win32的API实现的--这就是cygwin的做法.

## MinGW与Cygwin的区别

- MinGW相比CygWin/gcc来讲，更加贴近win32。因为它几乎支持所有的Win32API。它所连接的程序，不需要任何第三方库即可运行。
- CygWin/gcc，其实这是两个东西。CygWin是一个让Windows拥有Unix-like环境的软件。而gcc就是安装在CygWin上的编译器。
- CygWin/gcc与MinGW的最大区别在于：使用CygWin/gcc可以在Windows下调用unix-like的API，（如fork,spawn,signals,select,sockets等）。也就是说Cygwin是运行在Windows下的，但是她使用的是Unix-like系统的函数和思想。由于这个区别，导致的结果就是用CygWin/gcc编译出来的程序可以无缝的运行在*nix环境下。但是如果调用了unix特有的API函数，在windows环境下不能正常运行，如果想在windows下正常运行的，就必须依赖cygwin1.dll，速度上会有些影响。
- cygwin大，mingw小
- cygwin编译后的exe需要cygwin1.dll作为支持，而mingw不需要就可以直接运行，因为有中间层所以cygwin慢，mingw快。
- cygwin包含的内容更全面，能编译通过的linux源文件更多，mingw的min是minimalist所以能编译通过的更少。但，不是全部，就是说别指望你可以把任何为linux写的源代码在cygwin或mingw编译通过并运行。
