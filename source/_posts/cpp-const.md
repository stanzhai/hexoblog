title: C++ const学习总结(不整理还真觉得有些乱)
date: 2013-08-21 12:24:52
categories: 学习
tags:
---

## const对象

可以将const对象，赋值给同类型的普通对象，当不能将普通对象，复制给const对象。

```
const int a = 3;
int b = a;  // ok
int c = 4;
a = c;      // error
```

<!--more-->

## const形参

如,`void fcn(const int i) {}`,实参传递时以副本的形式传递,因此传递给fcn的对象既可以是const对象，也可以是非const对象。

在C语言中const形参或非const形参无区别。为了兼容C语言，编译器会将fcn的定义视为普通的int型，即`void fcn(int i) {}`。

** 仅当形参是引用或指针时，形参是否为const才有影响。

定义const来重载函数是合法的，编译器可以根据const确定调用哪一个函数：

```
Record lookup(Account&);
Record lookup(const Account&); // new function
```

## const指针,与指向const对象的指针

这种区别，可以采用从右向左的读法去判别。

```
// a是指向const int类型的指针
const int *a;
// a是常指针，执行int类型的对象。
int *const a;
```

** 允许把非const对象的地址，赋值给const对象的指针，如：

```
double dval = 3.14;
const double *cptr = &dval;   // 操作是ok的，但是不能通过cptr来修改dval的值
```