---
title: 设计模式之命令模式Command
date: 2017-12-04 15:00:22
tags: 设计模式
---

将一种请求封装成对象。

<!--more-->

代码示例：
```C++
class ICommand
{
	public:
	virtual void execute() = 0;
};

class switch{
	private:
	ICommand _closedCommand;
	ICommand _openedCommand;

	public:
	Switch(ICommand* closedCommand, ICommand* openedCommand)
	{
		_closedCommand = closeCommand;
		_openedCommand = openedCommand;
	}

	// close the circuit/power on
	void close()
	{
		_closedCommand->execute();
	}

	// open the circuit/power off_type
	void open()
	{
		_openedCommand->execute();
	}
};

class ISwitchable
{
	public:
	virtual void powerOn() = 0;
	virtual void powerOff() = 0;
};


class Light : public ISwitchable
{
	public:
	virtual void powerOn() override
	{
		cout << "The light is on." << endl;
	}

	virtual void powerOff() override
	{
		cout << "The light is off." << endl;
	}
};

//The Command for truning on the device - concreteCommand
class CloseSwitchCommand : public ICommand
{
	private:
	ISwitchable _switchable;

	public:
	void CloseSwitchCommand(ISwitchable* switchable)
	{
		_switchable = switchable;
	}

	virtual void execute() override
	{
		_switchable->powerOn();
	}
};

//The command for turning off the device - concreteCommand
class OpenSwitchCommand : public ICommand
{
	private:
	ISwitchable _switchable;

	public:
	OpenSwitchCommand(ISwitchabel* switchable)
	{
		_switchable = switchable;
	}

	virtual void execute() override
	{
		_switchable->powerOff();
	}
};


void main(int argc, char** argv)
{
	string argument = argc > 1 ? toUpper(argv[1]) : string();
	ISwitchable* lamp = new Ligth();

	//Pass reference to the lamp instance to each command
	ICommand* switchClose = new CloseSwitchCommand(lamp);
	ICommand* switchOpen = new OpenSwitchCommand(lamp);

	//Pass reference to instances of the Command objects to the switch
	Switch* pSwitch = new Switch(switchClose, switchOpen);

	if(argument == "ON")
	{
		pSwitch->close();
	}
	else if(argument == "OFF")
	{
		pSwitch->open();
	}
	else
	{
		cout << "Argument \"ON\" or \"OFF\" is required." << endl;
	}
}


```
