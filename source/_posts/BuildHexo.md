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
    repository: https://github.com/walkstep/walkstep.github.io.git
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
在blog目录下，使用vim命令编辑_config.yml，找到theme，修改为：

```
theme: next
```

