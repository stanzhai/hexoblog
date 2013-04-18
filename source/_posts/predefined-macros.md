title: ANSI C中的预定义宏
date: 2013-03-23
categories: 学习
tags: [C&C++]
---

## ANSI兼容的预定义宏

* `__DATE__`当前源文件的生成日期。使用示例：

	fprintf(stderr, "Temp (built on %s) by StanZhai", __DATE__);

<!-- more -->
* `__FILE__`当前源文件的名称
* `__LINE__`当前源文件的行号
* `__STDC__`表示是否完全符合ANSI C标准
* `__TIME__`当前源文件的最新生成时间
* `__TIMESTAMP__`当前源文件的上次修改日期和时间
