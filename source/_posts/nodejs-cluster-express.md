title: nodejs+cluster+express 构建高性能Web服务器
date: 2013-09-09 16:26:17
categories: 学习
tags: nodejs
---

## 前言

nodejs本身是单线程模型的，可以借助cluster模块实现多进程，增加程序在多核CPU机器上的性能表现，充分发挥nodejs的优势。

express算是nodejs中比较流行的Web开发框架了，假如你的网站使用的是express开发的，那么恭喜你，你只需添加少量的代码就可以实现多进程模式，重要的是这些进程可以共享一个端口哦。

<!--more-->

## 如何实现

这里直接贴出代码了，关键点请看注释：

```
/**
 * index.js 
 */
var cluster = require('cluster');
var numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  // Fork workers.
  for (var i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', function(worker, code, signal) {
    console.log('worker ' + worker.process.pid + ' died');
  });
} else {
  // Code to run if we're in a worker process
  require('./app');
  console.log('Worker ' + cluster.worker.id + ' running!');
}
```

以上代码中我们根据CPU的数量创建对应的进程数。假设我们原来的express启动代码为app.js那么只需要在工作者进程中执行require('./app')就可以了，改成多进程非常方便。

## 注意

cluster模块现在还处于实验阶段，不过使用ab测试了下，性能比单进程的提高了很多，大家可以自己去测下，这里就不给出测试数据了。