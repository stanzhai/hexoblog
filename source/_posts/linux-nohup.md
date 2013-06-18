title: linux 通过nohup使应用退出终端后继续运行
date: 2013-06-06 17:06:01
categories: 学习
tags: Linux
---

使用SSH登录到Linux，系统会创建一个Session表示本次会话，且在此Session上运行的程序会和Session关联，退出SSH会话，那么和Session相关联的程序也会关闭。

<!--more-->

我们可以通过nohup(no hang up)命令保证程序在session结束后继续执行。

**附：nohup命令参考**

**用途：**不挂断地运行命令。

**语法：**nohup Command [ Arg … ] [　& ]

**描述：**nohup 命令运行由 Command 参数和任何相关的 Arg 参数指定的命令，忽略所有挂断（SIGHUP）信号。在注销后使用 nohup 命令运行后台中的程序。要运行后台中的 nohup 命令，添加 & （ 表示”and”的符号）到命令的尾部。

**注意**

使用nohup时，在当shell中提示了nohup成功后还需要按终端上键盘任意键退回到shell输入命令窗口，然后在退出终端，nohup才会生效。
