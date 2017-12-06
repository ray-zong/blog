---
title: 设计模式之状态模式State
date: 2017-12-04 15:00:22
tags: 设计模式
---

定义状态量与行为的对应，当状态改变时，类对象的行为随着改变。

<!--more-->

代码示例：
```C++
class Statelike
{
	public:
	virtual void writeName(StateContext context, string name) = 0;
};

class StateLowerCase : public Statelike
{
	public:
	virtual void writeName(StateContext context, string name) override
	{
		cout << name.toLowerCase() << endl;
		context.setState(new StateMultipleUpperCase());
	}
};

class StateMultipleUpperCase : public Statelike
{
	private:
	int count = 0;

	public:
	virtual void writeName(StateContext context, string name) override
	{
		cout << name.toUpperCase() << endl;

		if(++count > 1)
		{
			context.setState(new StateLowerCase());
		}
	}
};


class StateContext
{
	private:
	Statelike myState;

	public:
	StateContext()
	{
		setState(new StateLowerCase());
	}

	void setState(Statelike newState)
	{
		myState = newState;
	}

	void writeName(string name)
	{
		myState.writeName(this, name);
	}
};

void main()
{
	StateContext* sc = new StateContext;

	sc->writeName("Monday");
　　　　sc->writeName("Tuesday");
        sc->writeName("Wednesday");
        sc->writeName("Thursday");
        sc->writeName("Friday");
        sc->writeName("Saturday");
        sc->writeName("Sunday");
}


```
