---
title: 设计模式之桥接模式Bridge
date: 2017-12-04 15:00:22
tags: 设计模式
---

解耦，抽象接口与实现分离。

<!--more-->

代码示例：

```C++

class IAbstractBridge
{
	public:
	virtual void CallMethod1() = 0;
	virtual void CallMethod2() = 0;
};

class AbstractBridge : public IAbstractBridge
{
	public:
	AbstractBridge(IBridge *bridge)
	{
		_bridge = bridge;
	}

	void CallMethod1()
	{_bridge->Function1();}

	void CallMethod2()
	{_bridge->Function2();}

	private:
	IBridge *_bridge;
};

class IBridge
{
	public:
	virtual void Function1() = 0;
	virtual void Function2() = 0;
};

class Bridge1 : public IBridge
{
	public:
	void Function1()
	{ cout << "Bridge1.Function1" << endl;}
	void Function2()
	{ cout << "Bridge1.Funciton2" << endl;}
};

class Bridge2 : public IBridge
{
	public:
	void Function1()
	{ cout << "Bridge2.Function1" << endl;}
	void Function2()
	{ cout << "Bridge2.Funciton2" << endl;}
};

```
