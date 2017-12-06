---
title: 设计模式之外观模式Facade
date: 2017-12-04 15:00:22
tags: 设计模式
---

将子系统中的不同接口封装成一个简单的接口，组合一组不同的子功能实现较复杂的功能接口。

<!--more-->

代码示例：
```C++
class CarModel
{
	public:
	void setModel()
	{
		cout << "CarModel-SetModel" << endl;
	}
};

class CarEngine
{
	public:
	void setEngine()
	{
		cout << "CarEngine-SetEngine" << endl;
	}
};

class CarBody
{
	public:
	void setBody()
	{
		cout << "CarBody-SetBody" << endl;
	}
};

class CarAccessories
{
	public:
	void setAccessories()
	{
		cout << "CarAccessories-SetAccessories" << endl;
	}
};

class CarFacade
{
	private:
	CarAccessories* _accessories;
	CarBody* _body;
	CarEngine* _engine;
	CarModel* _model;

	public:
	CarFacade()
	{
		_accessories = new CarAccessories;
        _body = new CarBody;
		_engine = new CarEngine;
		_model = new CarModel;
	}

	void createCompleteCar()
	{
		cout << "******Creating a Car******" << endl;
		_model->setModel();
		_engine->setEngine();
		_body->setBody();
		_accessories->setAccessories();

		cout << "********Car creation is completed.*********" << endl;
	}
};


void main()
{
	auto facade = new CarFacade();

	facade->createCompleteCar();
}

```
