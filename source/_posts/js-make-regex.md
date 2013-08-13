title: javascript动态构造正则表达式
date: 2013-08-02 15:41:58
categories: 学习
tags: js
---

在javascript中，我们构造正则表达的方式为`/regex expression/`的形式，如果要动态构造正则表达式该如何做呢？

<!--more-->

比如`/.*content.*/`中的content文本是不定的，我们可以通过如下方式去构造：

```
var content = "info";
eval("var reg = /.*" + content + ".*/");
// next, use reg ...
```