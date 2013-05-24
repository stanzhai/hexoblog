title: nodejs使用supervisor进行代码热部署
date: 2013-05-23 11:11:54
categories: 工具
tags: nodejs
---

最近使用nodejs做一个实时数据展现的系统，经常需要修改代码，调试。也就是反复的改代码，`node app.js`，`Ctrl+C`……

<!--more-->

这种反复的调试部署代码挺让人讨厌，我们可以使用supervisor进行代码热部署，它会监视文件的改动，一但代码被修改，supervisor会自动加载代码，进行重新部署。

**安装supervisor**

> npm install -g supervisor

**使用supervisor部署应用**

> 切换到代码所在目录，supervisor app.js(假设你的代码文件名就是app.js)