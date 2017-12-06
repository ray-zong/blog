---
title: 设计模式之访问者模式Visitor
date: 2017-12-04 15:00:22
tags: 设计模式
---

从对象结构中分离独立的算法(逻辑操作)，实现修改算法(逻辑操作)时，不改变对象结构。

<!--more-->

```C++
class NodeVisitor
{
	public:
	virtual void visitAssignment(AssignmentNode);
	virtual void visitVariableRefNode(AssignmentNode);

};


class TypeCheckingVisitor : public NodeVisitor
{
	public:
	virtual void visitAssignment(AssignmentNode);
	virtual void visitVariableRefNode(AssignmentNode);

};

class CodeGeneratingVisitor : public NodeVisitor
{
	public:
	virtual void visitAssignment(AssignmentNode);
	virtual void visitVariableRefNode(AssignmentNode);
};

class Node
{
	public:
	virtual void accept(NodeVisitor);
};



class AssignmentNode : public Node
{
	public:
	virtual void accept(NodeVisitor v)
	{
		v->visitAssignment(this);
	}
};


class VariableRefNode : public Node
{
	public:
	virtual void accept(NodeVisitor v)
	{
		v->visitVariableRef(this);
	}
};

```
