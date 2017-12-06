---
title: 设计模式之备忘录模式Memento
date: 2017-12-04 15:00:22
tags: 设计模式
---

保存一个对象的内部状态，在对象改变后，可以恢复到原先保存的状态。

<!--more-->

代码示例：
```C++
class Originator
{
	private:
	string _state;

	public:
	void set(const string &state)
	{
		_state = state;
		cout << "Originator:Setting state to " << _state << endl;
	}

	Memento* saveToMemento()
	{
		cout << "Originator:Saving to Memento." << endl;

		return new Memento(_state);
	}

	void restoreFromMemmento(const Memento* memento)
	{
		_state = memento->getSavedState();

		cout << "Originator:State after restoring from memento:" << _state << endl;
	}


};

class Memento
{
	private:
	string _state;

	public:
	Memento(const string &state)
	{
		_state = state;
	}

	string getSavedState()
	{
		return _state;
	}
};

void main()
{
	Originator org;
	org.set("state1");
	org.set("state2");

	Memento *mem = org.saveToMemento();

	org.set("state3");

	org.restoreFromMemento(mem);

	delete mem;
}

```
