---
title: 设计模式之模板方法Template Method
date: 2017-12-04 15:00:22
tags: 设计模式
---
在基类中定义一套算法框架，子类只能更改特定的实现细节。

<!--more-->

代码示例：
```C++
class AbstractClass
{
	public:
	void templateMethod()
	{
		function1();
		function2();
	}

	protected:
	virtual void function1()
	{
		cout << "invoke the function1 of AbstractClass" << endl;
	}
	virtual void function2()
	{
		cout << "invoke the function2 of AbstractClass" << endl;
	}
};


class SubClass1 : public AbstractClass
{
	protected:
	virtual void function1()
	{
		cout << "invoke the function1 of SubClass1" << endl;
	}

};

```
