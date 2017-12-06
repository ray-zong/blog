---
title: 设计模式之装饰模式Decorator
date: 2017-12-04 15:00:22
tags: 设计模式
---

动态地给一个对象扩展功能，而不是通过子类继承。

<!--more-->

代码示例：
```C++
//The Window interface class
class Window
{
	public:
	//Draws the Window
	virtual void draw() = 0;
	//Returns a description of the Window
	virtual string getDescription() = 0;
};

//Implementation of a simple Window without any scrollbars
class SimpleWindow : public Window
{
	public:
	virtual void draw() override
	{
		//Draw window
	}

	virtual string getDescription() override
	{
		return "simple window";
	}
};

//abstract decorator class - note that it implements window
class WindowDecorator : public Window
{
	protected:
	Window* _windowToBeDecorated; //the window being decorated

	public:
	WindowDecorator(Window* windowToBeDecorated)
	{
		_windowToBeDecorated = windowToBeDecorated;
	}

	virtual void draw() override
	{
		_windowToBeDecorated->draw(); //Delegation
	}

	virtual string getDescription() override
	{
		return _windowToBeDecorated->getDescription(); //Delegation
	}

};

//The first concrete decorator which adds vertical scrollbar functionality
class VerticalScrollBarDecorator : public WindowDecorator
{
	public:
	VerticalScrollBarDecorator(Window* windowToBeDecorated)
	: WindowDecorator(windowToBeDecorated)
	{}

	virtual void draw() override
	{
		WindowDecorator::draw();
		drawVerticalScrollBar();
	}

	virtual string getDescription()
	{
		return WindowDecorator::getDescription() + ", including vertical scrollbars";
	}

	private:
	void drawVerticalScrollBar()
	{
		//Draw the vertical scrollbar
	}
};


//The second concrete decorator which adds horizontal scrollbar functionality
class HorizontalScrollBarDecorator : public  WindowDecorator
{
	public:
	HorizontalScrollBarDecorator(Window* windowToBeDecorated)
	: WindowDecorator(windowToBeDecorated)
	{}

	virtual void draw() override
	{
		WindowDecorator::draw();
		drawHorizontalScrollBar();
	}

	virtual string getDescription()
	{
		return WindowDecorator::getDescription() + ", including horizontal scrollbars";
	}

	private:
	void drawHorizontalScrollBar()
	{
		//Draw the horizontal scrollbar
	}
};

void main()
{
	//create a decorated window with horizontal and vertical scrollbars.
	Window* decoratedWindow = new HorizontalScrollBarDecorator(new VerticalScrollBarDecorator(new SimpleWindow));

	//Print the window's description
	decoratedWindow->getDescription();
}


```
