---
title: 设计模式之策略模式Strategy
date: 2017-12-04 15:00:22
tags: 设计模式
---

封装不同的算法，使其独立于对象改变。

<!--more-->

代码示例：
```C++
class ICalculate
{
	public:
	virtual int calculate(int value1, int value2) = 0;
};

class Minus : public ICalculate
{
	public:
	virtual int calculate(int value1, int value2) override
	{
		return value1 - value2;
	}
};

class Plus : public ICalculate
{
	public:
	virtual int calculate(int value1, int value2) override
	{
		return value1 + value2;
	}
};

class CalculateClient
{
	private:
	ICalculate* _strategy;

	public:
	CalculateClient(ICalculate* strategy)
	: _strategy(strategy)
	{
	}

	~CalculateClient()
	{
		if(_strategy != nullptr)
		{
			delete _strategy;
		}
	}

	void setStrategy(ICalculate* strategy)
	{
		if(_strategy != nullptr)
		{
			delete _strategy;
		}

		_strategy = strategy;
	}

	int calculate(int value1, int value2)
	{
		assert(_strategy != nullptr);
		return _strategy->calculate(value1, value2);
	}
};


void main()
{
	CalculateClient* client = new CalculateClient(new Minus);

	cout << "Minus: " << client->calculate(7, 1) << endl;

	//change the strategy
	client->setStrategy(new plus);

	cout << "Plus: " << client->calculate(7, 1) << endl;

	delete client;
}

```
