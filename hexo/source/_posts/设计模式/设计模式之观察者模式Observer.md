---
title: 设计模式之观察者模式Observer
date: 2017-12-04 15:00:22
tags: 设计模式
---

定义一个管理类(观察者)，负责管理其他对某类状态感兴趣的对象(需注册及注销)。

<!--more-->

代码示例：

```C++
class Subject
{
	public:
	void attach(Observer *);
	void detach(Observer *);

	void notify()
	{
		for(Observer *obs : _observers)
		{
			obs->update();
		}
	}

	private:
	List<Observer *> _observers;
};

class Observer
{
	public:
	virtual void update(){}
};


class ObserverA : public Observer
{
	public:
	virtual void update() override
	{
		//do something
	}
};


void main()
{
	Subject client;

	Observer *obs = new ObserverA;
	client.attach(obs);

	client.notify();

	client.detach(obs);

	delete obs;
}

```
