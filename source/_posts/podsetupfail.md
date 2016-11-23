---
title: 解决pod setup缓慢及失败的另一种方式
date: 2016-11-23 09:04:16
tags: [ios, cocoapods]
categories: iOS
---
　　首先得清楚pod setup的作用原理是什么，pod setup本质其实就是将[https://github.com/CocoaPods/Specs](https://github.com/CocoaPods/Specs)上的项目克隆到/Users/用户名/.cocoapods/repos目录下，若此目录下已经有这个项目，使用pod setup命令则会将项目更新到最新的状态。<!--more-->
　　
　　前两天由于搜索不出一些第三方库的最新版本，需要更新cocoapods库，使用pod setup着实被坑了一大把(以前使用pod setup也还好)，因为我没有使用网线，用的是WiFi，网络一不稳定，就会导致pod setup失败，换成网线pod setup，下载速度也是10几k每秒。尝试过直接git clone仓库，尝试过使用coding和oschina上的镜像git clone，还尝试过fork Specs项目后，使用GitHub Desktop克隆，都是中途失败了，直接下载Specs的zip包，速度极其慢，而且zip包里不包含git文件，最终可能会导致第三方库不可用。
　　
　　最终解决的方法是下载zip包，将其解压后，从其他人那里拷贝一份git文件放到解压后的文件中，然后将整个文件拖到/Users/用户名/.cocoapods/repos目录下，详细步骤如下：
　　
　　1.从[https://github.com/CocoaPods/Specs](https://github.com/CocoaPods/Specs)下载zip包，目前是104M左右，用电脑下载会比较慢，不过使用使用手机流量下载就会快很多，我下载时使用4G网，速度是400k/s+(难道是因为流量访问没有被墙)，下载完成后把zip包传到mac上
　　
　　2.将你的mac设置为显示隐藏文件，解压zip包，这时文件夹名为Specs-master，将其改为master，其目录如下：![](http://oetpv7kic.bkt.clouddn.com/podsetupfail1.png)
　　
　　3.从其他iOS开发人员那里拷贝.git文件和.gitignore文件，如下图，路径是/Users/用户名/.cocoapods/repos/master![](http://oetpv7kic.bkt.clouddn.com/podsetupfail2.png)
　　
　　4.将拷贝好的两个文件拖到你的master文件夹中，将master文件夹拖到/Users/用户名/.cocoapods/repos路径下，这样你的目录将会是这样：![](http://oetpv7kic.bkt.clouddn.com/podsetupfail3.png)
　　
　　5.执行pod setup，等待其完成
　　
　　这样pod setup就完成了，希望能够帮到小伙伴们
　　
**欢迎吐槽，同时也欢迎转载，如转载请保留原文地址：
[https://walkstep.github.io/2016/11/23/podsetupfail](https://walkstep.github.io/2016/11/23/podsetupfail)**

**更新于2016年11月23日**
