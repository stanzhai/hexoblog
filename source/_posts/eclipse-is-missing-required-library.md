title: Eclipse 项目中一直提示 is missing required library
date: 2013-09-10 10:38:48
categories: 问题修复
tags: Eclipse
---

## 问题描述

今天接手同事的一个Java项目，发现将项目导入到Eclipse中有个红色的感叹号，编译能通过，也能调试运行，但是导出Runable的jar包就不行了，一直提示 is missing required library这个错误

<!--more-->

## 解决方案

看这个错误，很明显就是Build Path依赖包配置的问题，在项目的build path Library中存在重复并且冲突或者地址引用错误的jar包，常规解决方法为：

1. 右击项目------>选Build path------>再选Configure build Path；
2. 在右侧窗口中选择Library选项卡；
3. 在下面所列出的jar包中找到相互重复、冲突或者地址错误的jar包（一般有问题的jar包都显示黄色警告），删除之；
4. 点击确定关闭窗口，在eclipse或者Myeclipse自动重新build workspace后问题就解决了

**按照方法依旧提示那个错误，可是我缺失的包明明已经删掉了，我确信Build Path是正常的，真是怪异了**

按照使用Eclipse的经验，我知道Eclipse经常出现一些怪异的问题，我就重启了Eclipse，发现问题依旧。

然后又试了下将项目删除，然后重新导入到Eclipse中，惊奇的发现他好了！！！！

那个大红叉也不见了，导出jar包也OK了。

哎，虽然各种IDE多多少少会有些毛病，但是我想说“Eclipse你也太奇葩了吧”。
