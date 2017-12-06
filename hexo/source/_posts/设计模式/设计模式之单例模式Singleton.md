---
title: 设计模式之单例模式Singleton
date: 2017-12-04 15:00:22
tags: 设计模式
---

定义类的全局唯一示例对象，带有作用域的全局类对象，采用static实现。

<!--more-->

代码示例：
``C++
class Singleton
{
public:
    static Singleton *getInstance()
    {
        if(_singleton == nullptr)
        {
            //加锁
            if(_singleton == nullptr)
                _singleton = new Singleton;
        }
    }

    static void disInstance()
    {
        //加锁
        if(_singleton != nullptr)
        {
            delete _singleton;
            _singleton = nullptr;
        }
    }

private:
    Singleton();

    static Singleton *_singleton = nullptr;
};

```
