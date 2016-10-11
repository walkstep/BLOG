---
title: Mac上基于Github搭建Hexo博客
date: 2016-10-10 10:12:01
tags: [折腾, hexo]
categories: 折腾
---
　　无意之中，发现了基于NexT主题的Hexo博客，很喜欢这种大量留白的博客样式，于是自己着手搭建一个Hexo博客，现在分享一下搭建的过程。
***
#### 什么是Hexo
　　Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
<!--more-->
#### 安装
　　安装之前，首先得检查电脑里是否已将安装Node.js和Git。Node.js用来生成静态页面，Git用来将自己的博客内容提交到Github上。
　　如果电脑里已经安装了Node.js和Git，那么进入终端，执行npm命令：

```
$ sudo npm install -g hexo
```
如果你的电脑上没有安装所需要的程序，请查看[安装Node.js](#node)和[安装Git](#git)，根据步骤完成安装。已安装者可忽略。

<span id="node"></span>
#### 安装Node.js
　　安装Node.js最简单的方式就是下载[安装程序](https://nodejs.org/en/)来安装。

<span id="git"></span>
#### 安装Git
　　使用[Homebrew](http://brew.sh/)，[MacPorts](http://www.macports.org/)或下载[安装程序](https://sourceforge.net/projects/git-osx-installer/)安装，如果你的电脑上装了Xcode的话，就不用安装Git，因为Xcode自带Git。

#### 安装Hexo
　　所有必备的应用程序安装完成后，可使用npm安装Hexo。
```
$ sudo npm install -g hexo
```
#### 建站
　　进入终端，cd到一个目录，执行：

```
$ hexo init blog
```
blog是你用来存放hexo文件的文件夹，cd到blog下，执行：

```
$ npm install
```
新建完成后，blog文件夹的目录如下:

```
_config.yml    // 站点的配置信息
package.json   // 应用程序的信息
scanffolds     // 模板文件夹
source         // 存放用户资源的文件夹
themes         // 主题文件夹，Hexo会根据主题生成静态页面
```

执行如下命令，启动本地服务器：

```
$ hexo s
```
当命令执行完成后，打开网址[http://localhost:4000](http://localhost:4000)，如果看到下面的页面，那么Hexo就安装成功了(下面的图为盗用的图，自己在搭建的时候没有保存图，有空再补上)

![](http://oetpv7kic.bkt.clouddn.com/hexo4000.png)
下一步，就开始关联Github。
#### 关联Github
　　首先你得有一个Github账号，没有的话，请移步[Github](https://github.com/)注册。
##### 创建仓库
![](http://oetpv7kic.bkt.clouddn.com/createRepository.png)

![](http://oetpv7kic.bkt.clouddn.com/repositoryAddress.png)
　　终端里cd到blog文件夹下，使用vim命令编辑_config.yml，滑到最底部，将deploy里的内容改为如下：

```
deploy:
    type: git
    repository: https://github.com/walkstep/walkstep.github.io.git  // 这个地方填写自己的仓库地址
    branch: master
```
编辑完成后，按Esc键，输入：wq保存退出
执行如下命令生成静态页面：

```
$ hexo g
```
```
此时若出现如下报错：
ERROR Local hexo not found in ~/blog
ERROR Try runing: 'npm install hexo --save'

则执行命令：
npm install hexo --save

若无报错，请忽略
```
执行部署命令：

```
$ hexo d
```
```
若执行hexo d仍报错，则执行：
$ npm install hexo-deployer-git --save
```
然后再次还行hexo g和hexo d命令
hexo d成功后终端会提示你输入Github的用户名和密码，输入完成后打开网址[https://walkstep.github.io/](https://walkstep.github.io/),就能看到和打开[http://localhost:4000](http://localhost:4000)一样的界面。

为避免每次部署都需要输入Github用户名和密码，可以按以下步骤操作
##### 步骤1 检查SSH keys是否存在Github
　　执行如下命令，检查SSH key是否存在。如果有文件id_rsa.pub或id_dsa.pub，则直接进入步骤3将SSH key添加到Github中，否则进入下一步生成SSH key。

```
$ ls -al ~/.ssh
```
##### 步骤2 生成新的SSH key
　　执行如下命令生成public/private rsa key pair，注意将your_email@example.com换成你自己注册Github的邮箱地址。

```
$ ssh-keygen -t rsa -C "your_email@example.com"
```
默认会在相应路径下（~/.ssh/id_rsa.pub）生成id_rsa和id_rsa.pub两个文件。

##### 步骤3 将SSH key添加到Github中
　　Find前往文件夹~/.ssh/id_rsa.pub打开id_rsa.pub文件，里面的信息即为SSH key，将这些信息复制到Github的Add SSH key页面即可。
　　进入Github –> Settings –> SSH keys –> add SSH key
　　Title里任意添一个标题，将复制的内容粘贴到Key里，点击下方Add key绿色按钮即可。
#### 安装NexT主题
　　博客显示的样式是由Next主题来控制，首先下载主题，并将主题下载到thems/next文件夹下，可以使用git命令：

```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
你也可以在themes下建立一个next文件夹，然后将下载的NexT主题压缩包解压到next文件夹下

在blog目录下，使用vim命令编辑_config.yml，找到theme，修改为：

```
theme: next
```
> 需要注意的是，blog文件夹下会包含两个_config.yml，一个位于站点根目录下，也就是刚才创建的blog目录下，主要包含Hexo本身的配置；另一个位于主题目录下，主要用于配置主题相关的选项。

#### 在_config.yml中修改配置信息
注意冒号后面一定要有空格

##### 修改站点目录下的_config.yml
```
# Site
title: WalkStep's Blog                   // 站点标题
subtitle:                                // 站点副标题
description: First, crawl. Then, walk!   // 站点描述
author: WalkStep                         // 你的名字
language: zh-Hans                        // 站点语言
timezone:                                // 站点时区
avatar: /uploads/avatar.png              // 头像
```
```
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace:

如果要使代码高亮，需要将auto_detect改为true，还可以选择高亮主题，修改主题目录下的_config.yml会提到
```
```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/walkstep/walkstep.github.io.git
  branch: master
  
主题和部署上面已经说过
```
##### 修改主题目录下的_config.yml
```
menu:
  home: /
  categories: /categories
  about: /about
  archives: /archives
  tags: /tags
  #commonweal: /404.html
  
menu用来设置菜单，我这个地方设置了5个菜单选项，分别是首页、分类、关于、归档和标签，不让显示
的菜单选项可以用#注释掉
```
```
# Schemes
#scheme: Muse
scheme: Mist
#scheme: Pisces

schemes用来设置你使用NexT的样式主题，我使用的是Mist，这里有三种样式，选择一项，用#号注释另外两项
```
```
# Code Highlight theme
# Available value:
#    normal | night | night eighties | night blue | night bright
# https://github.com/chriskempson/tomorrow-theme
highlight_theme: night eighties

highlight_theme用来设置代码高亮的主题(默认是normal)
```
```
# Automatically Excerpt. Not recommand.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: true
  length: 150
  
这里用来设置是否显示[阅读全文]，将enable改为true(默认是false)，设置length的值来决定显示字符的长
度，不过Hexo推荐使用<!-- more -->来设置(的确后者更好用，你想显示多少就显示多少)，如果使用推荐的
方式的话，那么enable得设置为false，以免冲突
```
主题_config.yml基本就只需要设置这么多，如果你想使自己的站点功能更为完善的话，会设置里面的其他一些选项，后续会讲到

Hexo和主题配置设置完后，我们现在来说说怎样写文章和发布文章

#### 文章撰写和发布
　　终端里cd到blog目录下，执行：

```
$ hexo new "FirstArticle"
```
这时会在blog/source/_posts目录下生成FirstArticle.md文件，尽量不要使用中文来命令，方便以后设置超链接跳转。vim命令可以编辑文章，不过我想你应该不会使用的，不信你试试喽，推荐使用[MacDown](http://macdown.uranusjr.com/)，因为使用的是Markdown语法，所以你可以点击[这里](http://www.jianshu.com/p/q81RER)，还有[这里](http://www.markdown.cn/)，来简单学习一下Markdown的使用
编辑完成后，保存。在终端里依次执行：

```
$ hexo clean  // 清除缓存文件 (db.json) 和已生成的静态文件 (public)
```
```
$ hexo g     // 生成静态文件
```
如果你想在本地服务器上看一下效果，可以执行：

```
$ hexo s    // 启动本地服务器
```
打开网址[http://localhost:4000](http://localhost:4000)，你就可以看到你写的文章了，当你修改文章时，不需要反复执行生成静态文件和启动本地服务器命令，直接保存修改的文章，然后刷新浏览器就可以看到修改后的文章了
将文章部署到Github上, 执行：

```
$ hexo d    // 部署网点
```
打开你的Github个人主页，[https://walkstep.github.io](https://walkstep.github.io)，将walkstep替换成你的Github用户名，这样你就可以在Github上访问你的站点，看到你写的文章(部署到Github上有时稍慢，毕竟是免费的)

#### Tips
- [NexT主题使用](http://theme-next.iissnan.com/getting-started.html)
- [Next主题常见问题](http://theme-next.iissnan.com/faqs.html)
- [Hexo中文文档](https://hexo.io/zh-cn/docs/)
- [给博客添加文章阅读量统计功能](https://notes.wanghao.work/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud)
- [使用七牛云作为图床插入图片](http://developer.qiniu.com/article/kodo/kodo-first/quickstart.html)
- [备份Hexo博客源文件，再也不用担心换电脑](https://notes.wanghao.work/2015-04-06-%E5%A4%87%E4%BB%BDHexo%E5%8D%9A%E5%AE%A2%E6%BA%90%E6%96%87%E4%BB%B6.html)
- [新建文章时自动打开编辑器](https://notes.wanghao.work/2015-06-29-Hexo%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E6%97%B6%E8%87%AA%E5%8A%A8%E6%89%93%E5%BC%80%E7%BC%96%E8%BE%91%E5%99%A8.html)
- [添加多说评论系统](http://theme-next.iissnan.com/third-party-services.html), [Hexo使用多说教程](http://dev.duoshuo.com/threads/541d3b2b40b5abcd2e4df0e9)

**欢迎吐槽，同时也欢迎转载，如转载请保留原文地址：[https://walkstep.github.io/2016/10/10/BuildHexo](https://walkstep.github.io/2016/10/10/BuildHexo)**

**更新于2016年10月11日**



