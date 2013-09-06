title: C# 常用类库整理
date: 2013-09-06 18:55:50
categories: 学习
tags:
---

使用C#做开发算起来得有快4年的时间了，这里记录下自己常用的一些类库，及时更新，以便查看。

<!--more-->

## UI 类

* DevExpress：WinForm UI库，类型丰富，不过要收费
* Modern UI for WPF，仿Azune风格的UI库
* AvalonDock：WPF Dock控件，实现类似IDE的布局
* Ribbon：codeplex上找到的“Fluent Ribbon Control Suite”， “A Professional Ribbon control”

## AOP & IoC

* Sprint.NET: 源自Java平台的Spring，依赖注入框架
* PostSharp：静态植入的AOP框架，没怎么用过，看介绍功能强大，能节约不少代码
* Ninject：依赖注入框架

## 系统组件

* ShellIcon：提取系统文件关联图标
* SharpZipLib：zip压缩解压
* Newtonsoft.Json：Json序列化
* HtmlAgilityPack：非常好用的html dom操作类

## 文档操作类

* Aspose.Words：word文档操作
* dotnetCharting：强大的图表制作工具
* NPOI：Excel文档操作

## 数据库类

* Linq To Sql, EntityFramework，微软官方的就不多说了
* NHibernate：源自Java平台下的Hibernate，配置灵活，使用方便
* ActiveRecord，集合Nhibernate去用，简化配置文件的编写

## 测试

* NUnit: 单元测试用
* Moq：模拟数据

## 日志

* log4net：已经足够强大了，可以自定义自己的日志处理类，系统记录日志，必不可少。

## Web相关（有些算不着.net类库了，不过也记录下吧）

* CKEditor，CKFinder，html编辑器，文件浏览器
* easyui，uploadify，knockout.js，my97datepicker，highcharts，bootstrap都是前端的东西，不在一一介绍了
