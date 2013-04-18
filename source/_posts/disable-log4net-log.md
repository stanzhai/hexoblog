title: log4net屏蔽NHibernate日志
date: 2013-03-01
categories: 问题修复
tags: [log4net]
---

项目中使用了NHibernate作为数据库ORM组件，同时又使用了log4net作为日志组件。默认情况下，NHibernate会打印对数据库的操作日志，和我们自己写的日志消息混杂在一起，导致日志数量相当庞大，查看不方便。可以在log4net的配置文件中添加如下配置，来屏蔽NHibernate的日志输出：

	<logger name="NHibernate">
	  <additivity value="false" />
	</logger>
