---
title: 设计模式之组合模式Composite
date: 2017-12-04 15:00:22
tags: 设计模式
---

采用树形结构表示“部分-整体”关系，类用户使用一致的方式对待单个对象和组合对象。

<!--more-->

代码示例：
```C++
//Component
class Graphic
{
	public:
	//prints the graphic
	virtual void print();
};

//Composite
class CompositeGraphic : public Graphic
{
	private:
	List<Graphic*> childGraphics;
	public:
	void print()
	{
		for(Graphic *graphic : childGraphics)
		{graphic->print();}
	}

	//Adds the graphic to the composition.
	void add(Graphic *graphic)
	{
		childGraphics.add(graphic);
	}

	//Removes the graphic from the composition.
	void remove(Graphic *graphic)
	{
		childGraphics.remove(graphic);
	}
};

//Leaf
class Ellipse : public Graphic
{
	public:
	//Prints the graphic.
	void print()
	{
		cout << "Ellipse" << endl;
	}
}

//client
int main()
{
	//Initialize four ellipses
	Ellipse *ellipse1 = new Ellipse();
	Ellipse *ellipse2 = new Ellipse();
	Ellipse *ellipse3 = new Ellipse();
	Ellipse *ellipse4 = new Ellipse();

	//Initialize four composite graphics
	CompositeGraphic *graphic = new CompositeGraphic();
	CompositeGraphic *graphic1 = new CompositeGraphic();
	CompositeGraphic *graphic2 = new CompositeGraphic();

	//Composes the graphics
	graphic1->add(ellipse1);
	graphic1->add(ellipse2);
	graphic1->add(ellipse3);

	graphic2->add(ellipse4);

	graphic->add(graphic1);
	graphic->add(graphic2);

	//Prints the complete graphic (four times the string "Ellipse").
	graphic->print();

	return 0;
}


```
