---
title: Reveal集成指南
date: 2016-10-13 10:45:13
categories: 翻译
tags: [折腾, reveal]
---
　　很早就听过Reveal，对iOS Developer来说简直是一款神器。这次有时间打算体验一下Reveal，于是去官网看了下，没有中文使用文档，google了一下使用方法，博客里写得大同小异，有的简直就是一模一样，关键是有些还讲不清楚。于是，就产生了下面的文章。
***
　　目前计划翻译Reveal官网 [Konowledege Base/Getting Started](http://support.revealapp.com/kb/getting-started)下的八篇文章，因为水平有限，请大家多多指教，如有错误请帮忙指出。
　　以下内容翻译自[Reveal Integration Guide](http://support.revealapp.com/kb/getting-started/reveal-integration-guide)，官方更新于2016-9-23。文中链接暂时链接到官方英文文档，后续会慢慢更新。
<!--more-->

#### Reveal集成指南
要使用Reveal查看iOS或tvOS应用或者app，你必须先将其与Reveal Server框架链接。

我们支持以下几种集成方法：

- [使用CocoaPods](http://support.revealapp.com/kb/getting-started/integrating-using-cocoapods)是将Reveal Server集成到在设备和模拟器上运行的项目最简单的方法。它也适用于你团队的所有成员。
- [通过Xcode断点加载Reveal Server](http://support.revealapp.com/kb/getting-started/load-the-reveal-server-via-an-xcode-breakpoint)，你可以快速将Reveal Server集成到你的Simulator版本中，而无需更改项目中的任何内容。
- [将Reveal Server框架链接到你的应用程序](http://support.revealapp.com/kb/getting-started/integrating-reveal-via-linking)中就像我们的CocoaPods集成一样，但不依赖于CocoaPods。
- 如果你正在构建iOS app，我们会提供[其他说明](http://support.revealapp.com/kb/getting-started/integrating-reveal-with-app-extensions)，因为app的集成过程存在一些细微差异。

##### 集成Reveal最低要求
Reveal集成要求 macOS 10.11+，iOS 8+ or tvOS 9+ 和 Xcode 7+。

##### 从Reveal 1.x升级
捆绑框架的名称和路径在Reveal 2中已更改。

如果你在使用CocoaPods，请确保Podfile看起来和我们的建议类似，然后运行pod install。

如果你在使用其他集成方法中的一种，从Reveal 1.6.x更新为Reveal 2.x会是一个简单的过程，从你的项目中删除对libReveal.dylib或Reveal.framework的任何引用，然后按照说明进行操作链接框架或动态加载上面链接的框架。

##### 更多信息
- [手动停止和启动Reveal Sever](http://support.revealapp.com/kb/getting-started/manually-stopping-and-starting-the-reveal-server)。
- [从你的工程中移除Reveal](http://support.revealapp.com/kb/getting-started/remove-reveal-from-your-xcode-project)。

我们在我们的[在线Knowledge Base](http://support.revealapp.com/kb)提供了有关一些主题的更多信息。

##### 重要信息
- **不要将app链接到Reveal库。**Reveal暴露了你的app内部，可能会导致你的应用程序被苹果审查团队拒绝。Reveal仅用于内部开发和调试目的。

- **当iOS和tvOS app不是在最前的app，Reveal服务将自动停止。**当app重新打开时，它会自动重新启动。

- **Reveal支持对iOS 8.0及更高版本和tvOS 9.0或更高版本编译的应用程序进行检查。**iOS部署构建设置也必须在iOS项目中为“iOS 8.0”或更高版本。如果不是这样，你可能会看到链接错误。

- **Reveal可以使用Bonjour连接正在运行的iOS或tvOS应用程序。** 如果你在设备上运行iOS或tvOS应用程序，并希望使用Bonjour进行连接，则需要与Reveal Mac应用程序位于同一网络才能连接。 如果你在连接到你的应用程序时遇到任何问题，请检查你的防火墙和代理设置，以确保它们不会阻止通信，或者使用数据线将你的设备连接到你的Mac。

**欢迎吐槽，同时也欢迎转载，如转载请保留原文地址：
[https://walkstep.github.io/2016/10/13/RevealIntegrationGuide](https://walkstep.github.io/2016/10/13/RevealIntegrationGuide)**

**更新于2016年10月13日**
