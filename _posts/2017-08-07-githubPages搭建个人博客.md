---
layout: post
title: "github Pages 搭建个人博客"
subtitle: "personal Blog on github Pages"
date:   2017-08-07 13:57:00 +0800
author: "axmiao"
header-img: "img/post-bg-yading.jpg"
categories: jekyll update
catalog: true
tags: 
    - 前端
    - 博客
---

## 前言

作为一个伪程序媛一直有写一些技术分享的欲望，虽然文笔不佳，但总觉得只有拥有可以将技术整理成文字分享给需要的人的能力才算是一个真正的程序员。为此也曾经混迹在很多的博客平台，比如CSDN，网易博客等等。但是由于这些博客平台的样式没有办法自定义，一切都要按照既定的规则来写，所以总是没有那么强烈的归属感，后来看到了我神在github上自己搭建的博客，觉得逼格满满，不由得自己也想动手搭一个，奈何懒癌晚期，拖到现在才开始。虽然网上关于在github上的搭建博客的教程有很多，但我还是想要把我经历的过程写下来，与你分享。  

在最前面我要声明一点，我所用的电脑是Windows系统的，所以下面的介绍都是针对于Windows系统而言。

首先我们还是要对github，pages，jekyll做一些简单的介绍。

### github 及 pages 功能

github号称程序界的Facebook，自2008年上线以来就有着极高的人气，许多重要的项目都托管在这个平台上面。github仅支持以git作为唯一的版本库格式进行代码托管。

作为一个具有版本管理功能的代码仓库，每一个托管在github上的项目都有一个主页，在主页中列出项目的源文件，当一个新手程序员面对一大坨源码的时候，可能会不知道要从何入手，为此github推出了pages功能，允许用户自定义项目的首页。下面的图片就是正常的项目主页截图与pages生成的主页截图。  
![项目主页](/img/post_pic_03/post_03_01.png "项目主页")
![pages生成的项目主页](/img/post_pic_03/post_03_02.png "pages生成的项目主页")
**因此github可以被认为是用户编写的，托管在github上的静态网页。**  

github提供了模板，允许在站内生成网页，同时允许用户自己编写网页并上传，在生成pages的时候，上传的代码会经过jekyll程序的再处理，因此在github上搭建个人博客的步骤为：在本地编写符合Jekyll规范的网页源码，上传到github，再由github生成并托管整个网站。

### Jekyll

在刚才对githubpages的介绍中，提到了页面源码在生成pages时要经过Jekyll程序的加工处理，那么jekyll程序又是什么呢？

Jekyll是一个静态站点生成器，他会根据网页的源码生成静态文件，不需要数据库支持，其内部提供了变量，模板，插件等功能，他遵循liquid模板语言，实际上可以用来编写整个网站，jekyll生成的页面可以免费部署在github上，并且可以绑定自己的域名。之后的某篇文章会有针对性的介绍jekyll。

### github 搭建个人博客的优缺点

优点：
  * 免费且不限流量；
  * 享受git的版本管理功能，不用担心文章遗失；
  * 只需要用自己喜欢的编辑器写文章就好，其他的都丢给github处理就好；

缺点：
  * 需要一定的基础，懂一些git和网页开发的知识；
  * 只能生成静态网页，动态功能需要使用外部服务器，比如评论功能可能需要使用到disqus；
  * 不适合大型网站，由于没有用到数据库，没运行一次就需要遍历全部的文本文件，网站内容越多生成需要的时间就越长。


## 正文

这次我选择搭建博客的托管平台是github。其实coding也是可以的，但是coding的pages功能貌似需要成为人民币玩家才能用，穷码农一个打算节省些开销（请自行幻想一只委屈脸的喵）.  
不过好在github上的pages功能是免费的。只是github是国外的网站，网络状况不好的时候打开会有点卡。但除去这个缺点之外，在github上搭建博客真的是一个看起来高大上的行为。而且如果你是一直小菜鸟，当你想要去一家新公司面试的时候，如果你在github上有自己的账号，并且有一些托管在其上的开源项目，也是很重要的加分项。下面我们就具体的介绍搭建的一些细节。


### 准备工作

首先我们要在我们的电脑上安装git，ruby，gem，并通过gem安装jekyll并且有github账号。我们先来说明每一项的安装。

1.git  

git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目.[git的下载地址](https://git-scm.com/downloads)。下载对应自己电脑的版本到本地之后一路next就可以完成安装了。安装成功后在命令行中输入 
```
git --version
```
如果输出git的版本号，说明git安装成功。

2.ruby  

ruby是一种简单快捷的面向对象（面向对象程序设计）脚本语言。Jekyll的运行需要ruby语言环境的支持，Windows系统中通过ruby-installer来安装ruby。[ruby-installer下载地址](https://rubyinstaller.org/downloads/)。如下图，在粉色方框区域选择对应自己系统版本的installer下载到本地。然后依旧是一路next直到安装结束，其中比较重要的是第二张图中选项，前两项要选上，尤其是第一项，一定要把ruby加入到环境变量中。不然还要在安装之后手动添加
![installer-list](/img/post_pic_03/post_03_03.png "installer-list")
![installer-setting](/img/post_pic_03/post_03_04.png "installer-setting")
安装完成之后依旧是命令行中通过查看版本号来验证是否安装成功。代码如下：
```
ruby -v
```

3.gem

gem是集成在ruby中的包管理系统，类似于npm。当安装完ruby的时候一般gem已经自动安装，同样的也是通过在命令行中查看gem版本号来确定gem是否已经被安装好了，代码如下：
```
gem -v
```

4.通过gem安装jekyll
