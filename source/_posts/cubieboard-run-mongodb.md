title: cubieboard上运行MongoDB
date: 2013-07-28 15:07:44
categories: 
tags: cubieboard
---

本人用的cubieboard2，跑的LUbuntu，直接通过`apt-get install mongo-server`安装mongodb运行会报错误，从官网下载源码，编译时会报不支持Arm处理器的错误。

<!--more-->

于是乎，从网上搜索mongodb arm平台的相关信息，找到了<https://github.com/skrabban/mongo-nonx86> ,使用git clone其代码，安装scons进行编译，经过数小时漫长的等待，终于编译完了，看其生成的mongodb文件有120M，妈呀，这也太大了吧，都快赶上一个系统了，试着运行了一下倒是没有报错，关键是这个可执行文件确实大了点，感觉很不爽。

继续找资料发现github上还有个mongo-arm的开源项目<https://github.com/wtfuzz/mongo-arm>，后来又找到了编译好了的bin包<https://s3.amazonaws.com/wtfuzz/mongo-arm/mongodb-linux-armv7l-2.4.1.tgz>，由于编译比较慢，我就直接下载了bin包，解压后发现mongod只有9.8M，心中暗喜，这还差不多嘛，俺的cubieboard上本来就没有多少空间。创建了db文件夹，试着运行了下`mongod --dbpath=db`，又报了如下的错误：

```
Assertion: 14043:clear tmp files caught exception exception: locale::facet::_S_create_c_locale name not valid
exception in initAndListen: 14043 clear tmp files caught exception exception: locale::facet::_S_create_c_locale name not valid, terminating
```

好吧，没有看过mongodb的源码，继续查找解决方案，原来是local设置的问题，使用`export LC_ALL="C"`去除本地设置，在运行mongodb就可以了。

有图有真相(真是好折腾啊)：

![在cubieboard2上运行mongodb](/images/cubie-mongodb.png)

## 参考资料

1. mongodb arm： <https://jira.mongodb.org/browse/SERVER-1811>
2. locale::facet::_S_create_c_locale name not valid解决方案：<http://blog.csdn.net/lawrencesgj/article/details/8743778>
