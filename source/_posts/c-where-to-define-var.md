title: C语言变量定义的位置
date: 2013-06-18 19:32:30
categories: 学习
tags:
---

N久没有捣鼓C语言的东西了，今天打算用C写个大文件预览的工具，结果遇到了一堆低级的语法问题。其中一点是关于变量定义位置的问题：

<!--more-->

这里重新整理下有关C语言定义变量的知识：

以花括号{}给包围起来的代码段称为block(我不知道它的准确中文翻译是不是叫模块),只要在block开始的地方定义变量就不会错,且该变量的作用域和生存期(除了static限定)只在该block里,且该变量可以屏蔽block外的变量.譬如在block外已经有一个变量名为a = 1的int变量,在block里允许定义一个同名的变量int a = 2,但在block里试着用printf打印的话,会发现printf("%d", a)结果是2.这就叫做屏蔽外面的变量!