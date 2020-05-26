---
title: Node.js环境的安装与配置
date: 2020-05-25 10:27:58
categories: tools
tags: 
- Node.js 
- npm
---
&emsp; &emsp; Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台, 对于前端开发人员来说基本上必不可少，但对于非前端开发的人员来说就没有那么熟悉了，比如我；但由于某些原因，最近用到了Node.js下的npm包管理器，所以还是总结一下；

## Node.js安装

&emsp; &emsp; 这一步很简单，基本上没什么坑，如果还不了解的话可以访问地址：<a href = "https://www.runoob.com/nodejs/nodejs-install-setup.html">Node.js 安装配置</a>；

## npm修改全局默认路径

&emsp; &emsp; 为什么要修改全局默认路径呢？因为npm默认下载到c盘，如果你不想c盘迟早被装满的话，那么最好修改一下全局默认路径；主要分为以下几步：

1. 在node目录下建了两个文件夹分别叫node_global和node_cache。同时在cmd中运行以下命令：

``` 
npm config set cache "D:\node\node_cache"
npm config set prefix "D:\node\node_global"
```

2. 修改npm文件夹下的npmrc文件，打开修改里面的内容，原来的内容删掉，写入：

``` 
prefix=D:\node\node_global
cache=D:\node\node_cache
```

3. 设置环境变量，主要涉及两个变量：

> 在用户变量里面新建明为PATH的变量，值为D:\node\node_global, 这个值是你在步骤一种新建的文件夹的路径。
> 在系统变量里面新建一个叫NODE_PATH的变量，值为D:\node\node_global\node_modules

&emsp; &emsp; 为什么要设置环境变量呢？第一个变量是为了使我们能够在任何时候使用 npm -g 都能定位到该文件夹，第二个是为了任何时候我们使用npm下载的管理包都能够被全局识别；
