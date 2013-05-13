title: Windows平台安装Redmine2.2.x
date: 2013-01-30
categories: 工具 
tags: [Redmine, 项目管理]
---

## 安装准备

1. [下载RubyInstaller和Development Kit](http://rubyinstaller.org/downloads/)
2. 下载[MySql](http://www.mysql.com/downloads/mysql/)
3. 下载[mysql-connector-c-noinstall-6.0.2-win32.zip](http://cdn.mysql.com/Downloads/Connector-C/mysql-connector-c-noinstall-6.0.2-win32.zip)
4. 下载[Redmine](http://rubyforge.org/frs/?group_id=1850)
<!-- more -->

## 开始安装

### 1. 安装Ruby

执行RubyInstaller，一路next，安装即可

### 2. 安装DevKit

执行DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe，解压到：C:\DevKit

Win+R ,输入cmd，回车，cd C:\DevKit

	ruby dk.rb init
	ruby dk.rb install

即可完成DevKit的安装。

### 3. 安装Mysql

没啥可说的，一路next安装即可。

### 4. 向Ruby添加Mysql驱动文件

解压mysql-connector-c-noinstall-6.0.2-win32.zip，将lib目录中的libmysql.dll复制到C:\Ruby193\bin中即可。

### 5. 安装Redmine

将Redmine解压，路径最好不要包含中文。

控制台下进入Redmine解压后的目录。

接下来按照[官方文档](http://www.redmine.org/projects/redmine/wiki/RedmineInstall#Installation-procedure)进行配置。

**注意事项**

* 配置过程中只注意Windows和mysql的相关配置即可。
* 由于还没有安装rmagick，所以第二步的安装要使用如下命令：
> bundle install --without development test rmagick
* Step 7 - Database default data set，将set REDMINE_LANG=fr修改为set REDMINE_LANG=zh，即使用中文语言。
* 完成第9步配置就可使用ruby script/rails server webrick -e production启动网站了，访问http://localhost:3000，默认用户名admin密码admin

## 关于插件和主题

* 建议安装知识库knowledgebase插件（知识管理和积累）和Timesheet（工作量统计）插件。
* 他人推荐的[插件有](http://www.iteye.com/topic/910564)
* 主题个人比较喜欢[A1主题](http://redminecrm.com/pages/a1-theme)

