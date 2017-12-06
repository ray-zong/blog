---
title: 设计模式之抽象工厂模式Abstract Factory
date: 2017-12-04 15:00:22
tags: 设计模式
---
定义

抽象工厂模式是一系列相关的接口(工厂组成)封装成一个Factory。类用户在使用时，不需要关系具体Factory的实现，仅通过调用统一的Abstract Factory对象接口。

<!--more-->

代码示例：
```C++
class OSFactory
{
public:
    virtual createMenu() = 0;
    virtual createBarTool() = 0;
    //...
};

class MSFactory : public OSFactory
{
public:
    virtual createMenu(){
        //...
    }
    virtual createBarTool(){
        //...
    }
    //...
};

class MacFactory : public OSFactory
{
public:
    virtual createMenu(){
        //...
    }
    virtual createBarTool(){
        //...
    }
    //...
};
Enum System{Windows,Mac};
OSFactory *createFactory(System sys)
{
    switch(sys)
    {
        case Windows:
            return new MSFactory;
        case Mac:
            return new MacFactory;
        default:
            return NULL;
    }
}
OSFactory *pAbstractFac = createFactory(Windows);
pAbstractFac->createMenu();
pAbstractFac->createBarTool();

```

适用范围：

1.一个系统要独立于它的产品的创建、组合和表示时

2.一个系统要由多个产品系列中的一个来配置时

3.强调一系列相关的产品对象的设计以便进行联合使用时

4.提供一个产品类库，而只想显示它们的接口而不是实现时


优点：

1.分离抽象接口和具体实现类

2.易于交互产品系列

3.利于产品的一致性


缺点：

1.不容易支持新的产品，需要修改抽象工厂的接口


扩展

单例模式、工厂方法
