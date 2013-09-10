title: C++类的static成员
date: 2013-09-09 16:25:52
categories: 学习
tags: cpp
---

## 使用类的static成员的优点

1. static成员的名字在类的作用域中，可以避免与其他类的成员或全局对象名字冲突。
2. 可以进行封装。static成员可以是私有成员，而全局对象不可以。
3. 阅读程序可以看出static与类的关联，可以清晰的表明程序员的意图。

<!--more-->

## 定义static成员

在成员关键字前加上static将成员设为static，static成员遵循正常的公有/私有访问规则。

例如：

```
class Account {
public:
  void applyint() { amount += amount * interestRate; }
  static double rate() { return interestRate; }
  static void rate(double);     // sets a new rate

private:
  double amount;
  static double interestRate;
  static double initRate();
};
```

## static成员函数

Account类有2个rate的static成员函数，其中一个在类的内部定义，当我们在外部定义static成员时，无须重复指定static保留字，该保留字只出现在类定义体内部的声明处。

```
void Account::rate(double newRate)
{
  interestRate = newRate;
}
```

**static函数没有this指针**

static成员不是任何对象的组成部分，所以static成员函数不能声明为const（毕竟，将成员函数声明为const就是承诺不会修改该函数所属的对象，而static成员不属于对象）。

static成员函数也不能被声明为虚函数。

## static数据成员

static数据成员可以声明为任意类型，如常量，引用，数组，类类型等。

**定义，初始化方法**

static数据成员必须在类定义体的外部定义（正好一次）。static数据成员是在定义时初始化，而普通的数据成员是通过构造函数进行初始化。

```
// Account.cpp
double Account::interestRate = initRate();  
// initRate() is a private static method of Account
```

以上代码需要注意的是：interestRate定义在Account类的作用域中，因此可以直接访问该类的私有成员。

**const static成员**

一般而言，类的static成员和普通数据成员一样，不能再类的定义体中初始化。但是const static则是个例外。

```
class Account {
private:
  static const int period = 10; // const static数据成员可以在类中进行初始化。
  // 虽然已经在这进行初始化了，当时还必须在类外进行定义
};
```

## static成员不是类对象的组成部分

static数据成员的类型可以是该成员所属的类类型。非static成员被限定声明为其自身类对象的指针或引用，如：

```
class Bar {
private:
  static Bar mem1;  // ok
  Bar *mem2;        // ok
  Bar mem3;         // error
};
```

## 使用类的static成员

可以通过作用域操作符从类直接调用static成员，或者通过对象、引用或指向该类类型对象的指针间接调用。

**注意：在C#中只能通过类名去使用static成员哦**

## 参考文献

《C++ Primer中文版（第4版）》