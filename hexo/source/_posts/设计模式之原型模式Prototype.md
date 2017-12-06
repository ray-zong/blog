---
title: 设计模式之原型模式Prototype
date: 2017-12-04 15:00:22
tags: 设计模式
---

Prototype是创建型模式，通过copy原有的对象创建新的对象。


运行时创建对象

<!--more-->

代码示例：
```C++
class Base
{
public:
    virtual Base*clone() = 0;
};

class Derived : public Base
{
public:
    virtual Base *clone()
    {return new Derived(*this);}
};

```
