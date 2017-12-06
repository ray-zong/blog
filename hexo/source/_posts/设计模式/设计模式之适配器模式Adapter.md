---
title: 设计模式之适配器模式Adapter
date: 2017-12-04 15:00:22
tags: 设计模式
---
将一个类接口封装成另一类接口。

<!--more-->

代码示例：
```C++
class Adaptee
{
	public:
	void SpecificRequest();
};

class Adapter
{
public:
    void Request()
	{Adaptee::SpecificRequest();}
};
```

1.底层模块为上层模块提供接口，当底层模块改变时，不希望重新定义上层模块，在中间添加一层Adapter模块进行对接；

2.不同的类适配到一个算法，例如：iterater
