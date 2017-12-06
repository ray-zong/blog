---
title: 设计模式之构建模式Builder
date: 2017-12-04 15:00:22
tags: 设计模式
---

构建与实现（样式、表示）分离，一个构建逻辑对应不同的实现（样式、表示）。

<!--more-->

代码示例：

```C++
class Builder
{
    public:
        virtual void buildePartA(){}
        virtual void buildePartB(){}
        virtual void buildePartC(){}
};


class Product1 : public Builder
{
    public:
        virtual void buildePartA()
            {cout << "build part A of the Product1" << endl;}
        virtual void buildePartB()
            {cout << "build part B of the Product1" << endl;}
        virtual void buildePartC()
            {cout << "build part C of the Product1" << endl;}

}


class Product2: public Builder
{
    public:
        virtual void buildePartA()
            {cout << "build part A of the Product2" << endl;}
        virtual void buildePartB()
            {cout << "build part B of the Product2" << endl;}
        virtual void buildePartC()
            {cout << "build part C of the Product2" << endl;}
}

```
