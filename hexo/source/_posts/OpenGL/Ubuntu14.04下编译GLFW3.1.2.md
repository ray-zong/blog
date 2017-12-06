---
title: Ubuntu14.04下编译GLFW3.1.2
date: 2016-04-07 19:00:00
tags: OpenGL
---

[glfw](http://www.glfw.org/download.html)

<!--more-->

1.安装cmake：sudo apt-get install cmake

2.安装缺省库文件：（1）sudo apt-get install libxrandr-dev

                                  （2）sudo apt-get install libxi-dev

                                  （3）sudo apt-get install libxinerama-dev

                                   (4)sudo apt-get install libxcursor-dev

                                   (5)sudo apt-get install doxygen(可选，可生成HTML文档)

3.执行cmake：cmake .

4.编译源文件：make

5.将include、lib放在默认目录（usr/local）：sudo make install

-- Install configuration: ""
-- Installing: /usr/local/include/GLFW
-- Up-to-date: /usr/local/include/GLFW/glfw3.h
-- Up-to-date: /usr/local/include/GLFW/glfw3native.h
-- Up-to-date: /usr/local/lib/cmake/glfw/glfw3Config.cmake
-- Up-to-date: /usr/local/lib/cmake/glfw/glfw3ConfigVersion.cmake
-- Up-to-date: /usr/local/lib/cmake/glfw/glfwTargets.cmake
-- Installing: /usr/local/lib/cmake/glfw/glfwTargets-noconfig.cmake
-- Up-to-date: /usr/local/lib/pkgconfig/glfw3.pc
-- Up-to-date: /usr/local/lib/libglfw3.a
