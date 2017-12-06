---
title: 设计模式之代理模式Proxy
date: 2017-12-04 15:00:22
tags: 设计模式
---

为类提供另一个代理类，控制对该类的访问。

<!--more-->

代码示例：
```C++
class ICar
{
	public:
	virtual void driveCar() = 0;
};


class Car : public ICar
{
	public:
	virtual void driveCar() override
	{
		cout << "Car has been driver!" << endl;
	}
};

class ProxyCar : public ICar
{
	public:
	int _driver_age;
	ICar* _realCar;

	public:
	ProxyCar(int driver_age)
	: _realCar(new Car())
	, _driver_age(driver_age)
	{
	}

	~ProxyCar()
	{
		delete _realCar;
	}

	virtual void driverCar() override
	{
		if(_driver_age <= 16)
		{
			cout << "sorry, the driver is too young to drive." << endl;
		}
		else
		{
			_realCar->driverCar();
		}
	}
};

void main()
{
	ICar* car = new ProxyCar(16);
	car->DriverCar();
	delete car;

	car = new ProxyCar(25);
	car->DriverCar();
	delete car;
}

```
