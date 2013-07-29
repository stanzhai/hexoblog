title: 将cubieboard配置成远程下载服务器
date: 2013-07-28 16:27:46
categories: 
tags: cubieboard
---

保证cubieboard使用的是LUbuntu，安装aria2`apt-get install aria2`

<!--more-->

启动aria2，启用rpc，并设置下载目录，启动参数如下：

```
aria2c --enable-rpc --rpc-listen-all --rpc-allow-origin-all --file-allocation=none --max-connection-per-server=3 --max-concurrent-downloads=3 --continue --dir=/home/stan/nande/downloads
```

给chrome安装插件YAAW，并设置YAAW，JSON-RPC的地址(我的cubieboard的ip为192.168.2.103)，如下图所示：

![设置aria2的json-rpc地址](/images/aria-yaaw.png)

设置好以后，如果没有在页面出现：Error: Internal server error，这时下载应该就OK了。

## 附，aria2的参数介绍

#最大同时下载数(任务数), 路由建议值: 3 
max-concurrent-downloads=10 
#断点续传 
continue=true 
#同服务器连接数 
max-connection-per-server=5 
#最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要 
min-split-size=10M 
#单文件最大线程数, 路由建议值: 5 
split=10 
#下载速度限制 
max-overall-download-limit=0 
#单文件速度限制 
max-download-limit=0 
#上传速度限制 
max-overall-upload-limit=10K 
#单文件速度限制 
max-upload-limit=0 
#断开速度过慢的连接 
#lowest-speed-limit=0 
#默认下载路径 
#dir=/home/stan/nande/downloads
#Log 
#log=aria2c.log 
#BT下载相关 
#启用本地节点查找 
bt-enable-lpd=true 
#添加额外的tracker 
#bt-tracker=<URI>,… 
#单种子最大连接数 
#bt-max-peers=55 
#强制加密, 防迅雷必备 
#bt-require-crypto=true 
#当下载的文件是一个种子(以.torrent结尾)时, 自动下载BT 
follow-torrent=true 
#BT监听端口, 当端口屏蔽时使用 
#listen-port=6881-6999 
#允许rpc enable-rpc=true 
#允许所有来源, web界面跨域权限需要 
rpc-allow-origin-all=true 
#允许非外部访问 
rpc-listen-all=true 
#RPC端口, 仅当默认端口被占用时修改 
rpc-listen-port=6800