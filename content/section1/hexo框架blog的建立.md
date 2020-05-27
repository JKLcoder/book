---
title: "hexo框架blog基于Github pages的搭建"
comments: true
share: true
date: 2020-05-24
toc: true
categories: tools
tags: hexo
---

&emsp; &emsp; 之前使用过csdn写过blog，但是之前自己糟糕的排版，渐渐打消了我坚持下去的动力，直到最近，开始使用vscode，然后又学了一下markdown，才让我产生了重新开始写blog的想法；
&emsp; &emsp; 一开始选用了Jekyll，但是Jekyll框架下实在很难找到一个吸引人的主题，之前做了一个，但感觉实在很一般，于是开始使用hexo；
&emsp; &emsp; 配置hexo的过程实在不是一个开心的过程，差不过花了一整天，一个周末，就这样莫名其妙的过去了一大半，所以我觉得有必要整理一下自己在这个过程中的探索；

## 创建github page Repositories

&emsp; &emsp; 创建一个Repositories，必须是username.github.io的形式，username是你的github账号用户名，主要起到github pages部署blog时的识别作用；
&emsp; &emsp; 然后选择Repositories对应的settings，选择Choose a theme, 在这一步我们可以下载一个初始的主题，验证一下是否github pages已经发布成功！

## 生成SSH添加到Github

&emsp; &emsp; 这一步主要是针对github新手而言，以下操作都在git bash完成：

* 设置电脑要关联的github账号

``` 
git config --global user.name "yourname"
git config --global user.email "youremail"
```

* 验证是否设置正确

``` 
git config user.name
git config user.email
```

* 创建ssh，一路回车

``` 
ssh-keygen -t rsa -C "youremail"
```

&emsp; &emsp; 一般会生成两个文件，id-rsa和id_rsa.pub，你只需要把id_rsa.pub中的内容加入到github账号中去就可以了，具体做法是：登录web端的github账号后，点击settings -> SSH keys -> New SSH key, 把id_rsa.pub中的内容复制进去即可；

* 验证是否成功

``` 
ssh -T git@github.com
```

&emsp; &emsp; 为什么需要ssh呢？简单来讲，就是一个秘钥，其中，id_rsa是你这台电脑的私人秘钥，不能给别人看的，id_rsa.pub是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。对于初次在当前电脑使用github同步代码，这一步必不可少；

## Hexo安装

&emsp; &emsp; Hexo建站当然需要安装Hexo，要安装Hexo那么首先又要安装npm[^1]，而安装npm通常我们通常通过安装Node.js环境安装，至于Node.js环境安装网上资料多如牛毛，这里就不再具体介绍；以下操作均在终端中进行；

``` 
npm install -g hexo-cli
```

&emsp; &emsp; 这里的-g是指全局安装，一般情况下，我们在安装了Node.js之后，需要设置一个全局安装路径：所有使用npm下载的软件包都会下到该路径下，同时该路径也需要加入到环境变量中，这样无论在任一路径下，打开终端，我们都可以使用相应命令；

## Hexo建站

&emsp; &emsp; 在想要建站的目录，打开终端，进行以下操作：

``` 
hexo init
npm install
```

&emsp; &emsp; 第一条命令用于建站文件初始化，第二条用于根据package.json文件下载建站相关依赖文件；
完成上述操作后，我们基本上就应经获取了就已经获取了如下目录:

``` 
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

&emsp; &emsp; _config.yml用于站点各种信息配置，我们可以在<a href="https://hexo.io/zh-cn/docs/configuration">链接</a>中获取详细信息，这里不再赘述；
&emsp; &emsp; _post文件是我们用于存储需要发布md文件的目录；
&emsp; &emsp; themes用于存储我们的主题，对于Hexo来说，next模板可能是最常用的了吧，我们只需要下载一套next模板，然后放入thems目录，同时修改_config.yml中为theme: next即可；
&emsp; &emsp; 在完成上述操作的情况下，我们基本上已经可以预览一下网站效果了，如果只是预览，那么可以使用：

``` 
Hexo server
```

&emsp; &emsp; 如果想使用调试模式，那么使用：

``` 
Hexo s --debug
```

&emsp; &emsp; 在前述步骤完全配置成功的情况下，那么我们将会看到：

> INFO  Hexo is running at http://localhost:4000 ; 

&emsp; &emsp; 点击对应网址，即可看到效果了；

## next模板

&emsp; &emsp; 关于next模板使用的大部分技巧，在这个<a href="http://theme-next.iissnan.com/getting-started.html">链接</a>中都可以找到答案，但是这里面提供的next下载模板虽然自由度比较大，但是对于那些并不擅长前段开发的人来说，想要快速使用，同时又要保证效果尚佳的人来说，还是有一定难度，所以我推荐使用在next模板上已经进行一定程度开发的成熟模板，对于程序员来说这也是一种可以接受的做法：尽量找轮子，而非造轮子；
&emsp; &emsp; 这是我使用的<a href ="https://github.com/WordZzzz/hexo-next">模板</a>，这个哥们效果做的还是很丰富的，以至于我不得不屏蔽了部分效果；
&emsp; &emsp; 需要注意的是，对于next模板中的大部分文件，我们都是不需要操作的，我们所有需要修改的都可以在该目录下的_config.yml中完成；

&emsp; &emsp; 但是也可能存在一些问题，下面是我碰到的一些，以及我所找到的解决方案：

* 在next配置文件中，选择了一些菜单项，例如分类、标签，但是在预览时，点击菜单异常；<a herf = "https://blog.csdn.net/mqdxiaoxiao/article/details/93644533">解决方案</a>
* next模板的网站访问过慢！<a herf = "https://www.jianshu.com/p/95a8a7f70457">解决方案</a>

## Hexo部署到github pages

&emsp; &emsp; 这里体现了Jekyll和Hexo一个很大的不同：前者其站点的基础建站文件和github page对应的仓库文件是一样的，我们甚至可以修好md后直接放到其_post目录，刷新网页即可看到更新；后者github page对应仓库的文件实际是建站文件经过hexo编译后上传的文件；
&emsp; &emsp; 所以对于Hexo，建议使用两个仓库，一个用于发布Github pages，另一个则用于同步基础的建站文件，每次建站仓库部署后发布到前者上面；
&emsp; &emsp; 所以，对于Hexo部署，是必不可少的重要一步，操作流程如下：

1. 安装hexo-deployer-git部署工具；

``` 
$ npm install hexo-deployer-git --save
```

&emsp; &emsp; 只要执行过一次即可；

2. 修改建站文件中的_config.yml;

``` 
deploy:
type: git
repo: https://github.com/YourgithubName/YourgithubName.github.io.git
branch: master
```

&emsp; &emsp; 要注意，一般必须要求在主分支部署；

3. 部署；

``` 
hexo clean
hexo generate
hexo deploy
```

&emsp; &emsp; 其中 hexo clean清除了你之前生成的东西，也可以不加。
&emsp; &emsp; hexo generate 顾名思义，生成静态文章，可以用 hexo g缩写；
&emsp; &emsp; hexo deploy 部署文章，可以用hexo d缩写；

&emsp; &emsp; 注意deploy时可能要你输入username和password，目前发现输慢了可能导致部署失败，所以手速很重要；需要注意的是，该操作尽量在git bash中进行，因为git bash已经登录了github账号，所以要求输入账号密码的可能性不大，如果使用其他终端，则基本上每次都需要输入；
&emsp; &emsp; 这个步骤在我们每次有新文章同步到blog中都需要执行，但是也可以使用简化版：

``` 
hexo d -g
```

&emsp; &emsp; 此时我们可以不用hexo clean，hexo d -g会识别增量渲染，不会全部重新生成，感觉更安全一点；

## 关于hexo中的其他问题

* hexo中markdown的语法区别；
1. 使用```标记代码块时，后面一个不要加空格，否则代码块范围会识别错误，同时代码最好顶格处理，否则可能会莫名其妙的对不齐；
2. 写脚注时，\[^1]：中的冒号必须是中文的，否则显示异常；

* 在hexo中显示图片；

&emsp; &emsp; _config.yml中设置：

``` 
post_asset_folder: true
```

&emsp; &emsp; 使用下述命令创建文章：

``` 
hexo new [layout] <title>
```

&emsp; &emsp; 则会自动创建一个文件夹。这个资源文件夹将会有与这个文章文件一样的名字。这一步也可以手动完成，之后把我们的图片资源放在这个目录下；
&emsp; &emsp; 使用下述方式引用图片资源：

``` 
{% asset_img 1.jpg %}
```

&emsp; &emsp; 然后我们就能在blog中看到图片的正常显示了；
&emsp; &emsp; 需要注意的是：这里并不需要我们把资源目录也加进去，因为hexo会把这些文件编译后部署到github pages，如果我们打开目录将会发现图片已经和生成的对应html放在同一目录下了；

参考：<a herf="https://blog.csdn.net/sinat_37781304/article/details/82729029">链接1</a>

[^1]: 软件包管理器，用于下载一些软件包, 包括Hexo；
