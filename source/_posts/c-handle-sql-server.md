title: C++操作SQL Server数据库
date: 2013-03-19
categories: 问题修复
tags: [C++, SQLServer]
---

## 前期准备

1. 导入库

	#import "c:\Program Files\Common Files\System\ADO\msado15.dll" \
	no_namespace rename("EOF", "EndOfFile")
<!-- more -->

2. COM初始化（进程级使用只需初始化一次，如果在线程中使用，则在线程函数中每次执行初始化）

	::CoInitialize(NULL);

3. 关键变量

	_ConnectionPtr m_pConnection;

## 简单实例

	m_pConnection.CreateInstance("ADODB.Connection");
	m_pConnection->Open(connStr,"","",adModeUnknown);
	char query_cmd[500] = {0};

	...

	m_pConnection->Execute(query_cmd,NULL,1); //用Execute执行sql语句来执行写入
	m_pConnection->Close();

## 常见错误`_com_error`的处理

使用try catch语句捕获异常信息，确定错误原因：

	try
	{
		m_pConnection->Execute(query_cmd,NULL,1);
	}
	catch(_com_error& e)
	{
		MessageBox(NULL, e.Description(), e.ErrorMessage(), MB_ICONERROR);
		return;
	}
