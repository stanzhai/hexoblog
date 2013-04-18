title: SQL Server，使用Union合并2表数据
date: 2013-03-21
categories: 学习
tags: [SQL]
---

## 表结构介绍

* A表：Id, Title, Type, Date
* B表：Id, Name, Date

## 期望合并后的表结构为
<!-- more -->

合并表：RowId, DataId, Name, Date, Type

* RowId为Guid类型，标识唯一的一行信息
* DataId，对应原始表A或B中Id
* Name，对应A表Title或B表Name
* Date，A表或B表的Date
* Type, A表的Type，B表使用‘B’填充

## 使用Union合并语句

	Select Newid() as RowId, Id as DataId, Title as Name, Date, Type
		From A
	Union All
	Select Newid() as RowId, Id, Name, Date, 'B' From B

## 解释说明

* Union语句要求合并的表的列数相同，且列的类型相似;
* Newid()是SQL Server中自带的系统函数，用于生成GUID;
* 默认情况下Union会合并相同的行，Union All则不合并。
