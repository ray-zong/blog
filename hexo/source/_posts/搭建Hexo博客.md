---
title: 搭建Hexo博客
date: 2017-12-04 13:15:37
tags: 网站博客
---
采用[github](https://github.com/)和[Hexo](https://hexo.io/)搭建自己的博客.

<!--more-->

github提供免费的存储空间,Hexo是个博客框架，利用主题帮我们生成静态网页。

## 1.Hexo安装
[Hexo官方文档](https://hexo.io/zh-cn/docs)

#### (1)准备安装前的工作
- ** 安装Node.js **
[Node.js的官方网址](https://nodejs.org.en), 官网自动检测系统类型(x86或者x64)并提供两个软件版本(LTS和Current：LTS长期支持版本，Current为最新版本），按自己的喜好下载对应版本。
> **注意：**安装过程中，如果没有特定要求，直接点击【下一步】即可。

- ** 安装Git **
Git的官方网址：https://git-scm.com, 点击downloads，跳到下载页面，根据自己的系统下载最新版本。

 > **注意：**安装时，选择将可执行程序所在目录写入PATH环境变量。

#### (2)安装Hexo

1. 启动命令行终端，** 管理员权限 ** ；
2. 输入命令：`npm install -g hexo-cli`。


## 2.建站

1. 新建一个目录，例如：blog；
2. 进入新建的目录，输入命令：`cd blog`；
3. 启动hexo初始化(确保当前是空目录)，输入命令：`hexo init`（管理员权限）
4. 输入命令：`npm install`

## 3.博客配置

建站完成后，所在目录下会找到一个_config.yml文件，存放基本的配置信息。
详细的配置选项参考：https://hexo.io/zh-cn/docs/configuration.html

## 4.写新博客

创建一篇新的文章， 输入命令：`hexo new hello`。该命令会在目录\source\_posts生成hello.md文件，可以启动一个自己喜欢的编辑器打开文档进行写作。

文档采用Markdown格式。

## 5.文件转换为网页

博客写完并保存后，输入命令：`hexo generate`(简写：`hexo g`)。

## 6.本地浏览

- 输入命令： `hexo server`(简写：`hexo s`).
- 打开本地浏览器，访问网址： http://localhost:4000/

## 7.将网页部署到服务器

说明：我采用的是github。
输入命令： `hexo deploy`(简写：`hexo d`)。
