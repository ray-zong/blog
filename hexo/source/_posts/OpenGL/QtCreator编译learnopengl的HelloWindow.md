---
title: QtCreator编译learnopengl的HelloWindow
date: 2016-04-08 11:00:00
tags: OpenGL
---

[源码](http://www.learnopengl.com/#!Getting-started/Hello-Window)

<!--more-->

问题：

1、提示各种链接函数未定义（undefined reference to symbol "XXX@XXX"）

解决方案：在Qt的pro文件中添加LIBS += -ldl -lX11 -lXrandr -lXi -lXxf86vm -lXinerama -lXcursor

2、glewExperimental和glewInit()未声明或定义

解决方案：（1）下载glew并编译安装,路径为：https://sourceforge.net/projects/glew/files/?source=navbar

         （2）在Qt的pro文件中添加LIBS += -lGLEW（注：库名称的大小写依据安装库而定）
