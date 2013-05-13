title: sqlite 数据库错误,Corrupt
date: 2013-02-28
categories: 问题修复
tags: [sqlite]
---

最近由于程序在对sqlite数据库操作的时候频繁中止，导致操作数据表时出现如下错误：

	[System.Data.SQLite] ErrorCode: Corrupt
	ErrorMessage: The database disk image is malformed
	database disk image is malformed

<!-- more -->
参考网上的修复方法，首先使用sqlite命令工具将sqlite数据库导出为sql文件，然后基于sql文件重新生成新的数据库就可以了。方法如下：

	sqlite3 newsfeed.db .dump > newsfeed.sql
	sqlite3 newsfeed.db < newsfeed.sql

