---
title: 设计模式之工厂方法Factory Method
date: 2017-12-04 15:00:22
tags: 设计模式
---

定义一个单一创建接口，让子类实现不同对象的创建。

<!--more-->

代码示例：
```C++
//interface
class Product
{
};

//interface
class Creator
{
public:
    virtual Product *createProduct() = 0; //factory method
};

class Product1
{};

class Creator1 : public Creator
{
public:
    Product *createProduct()
    {
        return new Product1;
    }
};

```
