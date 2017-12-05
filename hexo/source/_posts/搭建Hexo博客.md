---
title: 搭建Hexo博客
date: 2017-12-04 13:15:37
tags: 其他技术
---
采用[github](https://github.com/)和[Hexo](https://hexo.io/)搭建自己的博客.

<!--more-->

github提供免费的存储空间,Hexo是个博客框架，利用主题帮我们生成静态网页。

## 1.Hexo安装
官方文档：https://hexo.io/zh-cn/docs

#### 准备安装前的工作
** 安装Node.js **
Node.js的官方网址：https://nodejs.org.en, 官网自动检测系统类型(x86或者x64)并提供两个软件版本(LTS和Current：LTS长期支持版本，Current为最新版本），按自己的喜好下载对应版本。
> ** 注意 ** 安装过程中，如果没有特定要求，请直接点击【下一步】

** 安装Git **
Git的官方网址：https://git-scm.com, 点击downloads，跳到下载页面，选择对应的系统下载最新版本。

> ** 注意 ** 安装时，选择将可执行程序所在目录写入PATH环境变量。

#### 安装Hexo

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


## Hexo命令
Hexo new [文件名] 创建新的文章名称
Hexo clean
Hexo g 生成静态文件

Hexo d 提交到git  repository

Hexo server 启动本地服务器。命令运行后，打开浏览器，地址栏输入：http://localhost:4000可以预览网页。
## 插件
注：
插件安装必须有系统管理员权限，即运行终端时，鼠标右击，点选【以管理员身份运行】。

RSS订阅插件
npm install hexo-generator-feed –-save

搜索站点(百度、Google等)收录
插件安装
npm install hexo-generator-sitemap –save
npm install hexo-generator-baidu-sitemap --save

运行命令hexo g后，会在public目录下生成sitemap.xml、baidusitemap.xml

百度站点平台（https://ziyuan.baidu.com/dashboard/index）

谷歌站点平台（https://www.google.com/webmasters/tools）


博客文档插入图片方法：

_config.yml中post_asset_folder设为true

安装插件
npm install hexo-asset-image –save

运行新建博客命令(hexo new)，会在/source/_posts目录下生成同名文件夹。
例如：
Hexo new image

Source/_posts目录下会新生成“image”文件夹。

将使用的图片放入文件夹中，在博客中引用方式：
![图片描述（可省略）](/文件夹名/你的图片名字.JPG)

例如：
![](image/1.jpg)

若使用网络图片直接将图片地址放入()即可，
例如：
![]( http://img07.tooopen.com/images/20170309/tooopen_sy_201188749612.jpg)


音乐插入
网易云音乐
网页上点击收听歌曲(如：成都)，点击左侧“生成外链播放器”，调整属性值。复制HTML代码，粘贴到博客中。
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=436514312&auto=1&height=66"></iframe>
