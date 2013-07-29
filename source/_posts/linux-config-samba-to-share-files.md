title: linux 配置samba服务来共享文件
date: 2013-07-28 12:49:14
categories: 学习
tags: Linux
---

1. 安装samba服务，`apt-get install samba`(我用的Ubuntu，如果是CentOS或者Fedora，把apt-get换成yum即可)

<!--more-->

2. 修改配置文件 `/etc/samba/smb.conf`,在最后添加
	
```
[share]
    comment = linux share
    # 修改成要共享的路径
    path = /home/stan/nande
    public = yes
    writable = yes
    printable = no
    write list = +staff
```

3. 重启smb服务`service smbd restart`