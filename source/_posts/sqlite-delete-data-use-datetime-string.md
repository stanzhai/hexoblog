title: sqlite按照日期删除数据是需要注意的地方
date: 2013-05-11 23:44:49
categories: 问题修复
tags:
---

## 注意点

在sqlite中按照日期删除数据时，日期格式必须是yyyy-MM-dd HH:mm:ss这种格式的。

如：

```
var conn = new SQLiteConnection("data.db");
conn.Execute("delete from bbs where publish >= ?", new[] { "2013-05-10" });
```

其中日期字符串不能写成"2013-5-10"，sqlite中和日期相关的其他操作同理。