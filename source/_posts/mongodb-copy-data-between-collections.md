title: mongodb从一个集合复制数据到另一个集合
date: 2013-06-05 11:07:41
categories: 学习
tags: mongodb
---

使用javascript，通过游标操作，代码如下：

<!--more-->

```
// 将BBS集合中的部分数据复制到BBS2集合中。
for (var data =  db.BBS.find({Publish: {$gte: new Date(2013, 3, 1)}}); data.hasNext();) {
  db.BBS2.insert(data.next());
}
```
