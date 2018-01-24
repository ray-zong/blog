---
title: system()函数危害
date: 2018-01-24 00:00:00
tags: C++
---
原文：http://www.cplusplus.com/articles/j3wTURfi/


<!--more-->

占用资源多

首先，你必须思考system()函数真正做什么：它不止运行一个进程，而是两个独立的进程以及返回一个退出的状态量给调用程序(系统主程序依据退出状态量判断调用是否成功).
[system函数手册](https://linux.die.net/man/3/system)详细描述system()函数的调用及返回值，少量资源存储错误状态。

system("PAUSE")仅仅是简单地等待终端输入，但是，却需要做许多工作。参见：http://www.gidnetwork.com/b-61.html

安全性差

使用system()函数，可能出现下列问题：
1.无效命令；
2.不能在所有平台运行；
3.无法防止恶意代码；
4.是一个独立的程序。

下列代码是控制台程序：
#include <stdio.h>
#include <stdlib.h>

#if defined(WIN32) || defined(_WIN32) || defined(__WIN32__) || defined(__TOS__WIN__) || defined(__WINDOWS__)
#define EDITOR "notepad"
#else
#define EDITOR "emacs"
#endif

int main(int argc, char **argv)
{
	printf("Now I'm going to start your text editor!\n");
	system(EDITOR);
	printf("Good-bye!\n");
	return 0;
}

Unix/Linux用户可能会遇到下列问题：
-系统没有安装emacs。
-不知道如何退出emacs。
-运行程序前，必须保证emacs的环境变量路径配置正确。

假设上述程序已经可以正确运行，在相同目录下运行下列代码：
#include <stdio.h>

int main(int argc,char ** argv)
{
	printf("Bwah, hah, hah, hah, hah!\n");
	return 0;
}

编译代码，并将其更改为"notepad.exe"或者"emacs"。

现在再次运行程序，会发生什么现象？

更加危险的是你有可能以管理员权限运行程序，这时，system调用的程序也具备管理员权限。

防病毒软件警告

如果你的软件用户安装某些防病毒软件，例如ZoneAlarm,Norton,McAfee等。运行软件时，这些防病毒软件可能提示软件有风险。

综上所述，避免使用system()。

如果你必须使用system(),建议先检测是否有有效的shell。
if(system(NULL)) then_I_can_safely_use_system();
else fooey();
