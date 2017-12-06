---
title: 设计模式之中介者模式Mediator
date: 2017-12-04 15:00:22
tags: 设计模式
---

将不同的对象间交互封装到一个对象中，类似黑板或者交互平台。

<!--more-->

代码示例：

```C++
class DialogDirector{
	public:
	virtual ~DialogDirector();

	virtual void showDialog();
	virtual void widgetChanged(Widget*) = 0;

	protected:
	DialogDirector();
	virtual void createWidgets() = 0;
};


class Widget{
	public:
	Widget(DialogDirector*);
	virtual void changed()
	{
		_director->widgetChanged(this);
	}

	virtual void handleMouse(MouseEvent& event);

	private:
	DialogDirector* _director;
};

class ListBox : public Widget{
	public:
	ListBox(DialogDirector*);

	virtual const char* getSelection();
	virtual void setList(List<char*>* listItems);
	virtual void handleMouse(MouseEvent& event);
};

class EntryField : public Widget{
	public:
	EntryField(DialogDirector*);

	virtual void setText(const char* text);
	virtual const char* getText();
	virtual void handleMouse(MouseEvent& event);
};

class Button : public Widget{
	public:
	Button(DialogDirector*);

	virtual void setText(const char* text);
	virtual void handleMouse(MouseEvent& event)
	{
		changed();
	}
};

class FontDialogDirector : public DialogDirector{
	public:
	FontDialogDirector();
	virtual ~FontDialogDirector();
	virtual void widgetChanged(Widget* theChangedWidget)
	{
		if(theChangedWidget == _fontList)
		{
			_fontName->setText(_fontList->getSelection());
		}
		else if(theChangedWidget == _ok)
		{
			//apply font change and dismiss dialog
		}
		else if(theChangedWidget == _cancel)
		{
			//dismiss dialog
		}
	}

	protected:
	virtual void createWidget()
	{
		_ok = new Button(this);
		_cancel = new Button(this);
		_fontList = new ListBox(this);
		_fontName = new EntryField(this);
	}

	private:
	Button* _ok;
	Button* _cancel;
	ListBox* _fontList;
	EntryField* _fontName;
};

```
