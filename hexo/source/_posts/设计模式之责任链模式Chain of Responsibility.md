---
title: 设计模式之责任链模式Chain of Responsibility
date: 2017-12-04 15:00:22
tags: 设计模式
---

将一种响应（请求）通过一条彼此关联的处理类处理。一般遵照由小到大的粒度。

<!--more-->

```C++

class PurchasePower
{
	protected:
	static final double _BASE = 500;
	PurchasePower* _successor;

	protected:
	virtual double getAllowable() = 0;
	virtual string getRole() = 0;

	void setSuccessor(PurchasePower* successor)
	{
		_successor = successor;
	}

	void processRequest(PurchaseRequest* request)
	{
		if(reuqest->getAmount() < getAllowable() )
		{
			cout << getRole() << "will approve $" + request->getAmount() << endl;
		}
		else if(_successor != nullptr)
		{
			_successor->processRequest(request);
		}
	}
};

class ManagerPPower : public PurchasePower
{
	protected:
	virtual double getAllowable() override
	{
		return _BASE * 10;
	}

	virtual string getRole() override
	{
		return "Manager";
	}
};

class DirectorPPower : public PurchasePower
{
	protected:
	virtual double getAllowable() override
	{
		return _BASE * 20;
	}

	virtual string getRole() override
	{
		return "Director";
	}
};

class VicePresidentPPower : public PurchasePower
{
	protected:
	virtual double getAllowable() override
	{
		return _BASE * 40;
	}

	virtual string getRole() override
	{
		return "Vice President";
	}
};


class PresidentPPower : public PurchasePower
{
	protected:
	virtual double getAllowable() override
	{
		return _BASE * 60;
	}

	virtual string getRole() override
	{
		return "President";
	}
};

class PurchaseRequest
{
	private:
	double _amount;
	string _purpose;

	public:
	PurchaseRequest(double amount, string purpose)
	{
		_amount = amount;
		_purpose = purpose;
	}

	double getAmount()
	{
		return _amount;
	}

	void setAmount(double amount)
	{
		_amount = amount;
	}

	string getPurpose()
	{
		return _purpose;
	}

	void setPurpose(string purpose)
	{
		_purpose = purpose;
	}
};

void main()
{
	ManagerPPower* manager = new ManagerPPower();
	DirectorPPower* director = new DirectorPPower();
	VicePresidentPPower* vp = new VicePresidentPPower();
	PresidentPPower* president = new PresidentPPower();

	manager->setSuccessor(director);
	director->setSuccessor(vp);
	vp->setSuccessor(president);

	try
	{
		while(true)
		{
			cout << "Enter the amount to check who should approve your expenditure." << endl;
			cout << ">" << endl;
			double d = 0;
			cin >> d;
			manager->processRequest(new PurchaseRequest(d, "General"));
		}
	}
	catch(...)
	{
		exit(1);
	}
}

```
典型的例子：界面帮助请求
