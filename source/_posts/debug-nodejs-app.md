title: 调试nodejs应用
date: 2013-06-26 14:43:36
categories: 学习
tags: nodejs
---

nodejs的调试方案有不少，可以使用内置的调试功能，在代码中添加debugger指令；可以使用Eclipse加V8调试，这里介绍的是本人比较喜欢的node-inspector调试方案。

<!--more-->

**首先全局安装node-inspector**

  npm install -g node-inspector

**以调试模式启动应用(app.js为例)**

  node --debug app.js

**启动node-inspector**

  $node-inspector &

**打开浏览器进行调试**

http://127.0.0.1:8080/debug?port=5858

使用Chrome调试前台的童鞋对这个东东应该比较熟悉吧。

在这里，可以方便的添加断点进行调试，也可以查看变量的值，很酷吧。

**注意事项**

如果无法显示脚本，可能是由于您的程序执行过快导致的，启动应用是使用`node --debug-brk app.js`，这样启动调试会在第一行代码断住，就可以看到脚本了。