---
author: hesicong
comments: true
date: 2007-02-08 22:09:17+00:00
layout: post
slug: bender-3d-demo
title: 弯管机3D DEMO
wordpress_id: 131
categories:
- 其他
tags:
- C++/CLI
- opengl
---


昨天经过一些努力，把弯管机的管子加到了弯管机上面。添加了一些手工的弯管步骤，做了一个DEMO。

最后在其他非开发机上面运行的时候，出现了“由于应用程序配置不正确，应用程序未能启动。重新安装应用程序可能会纠正此问题。”这个问题。上网查询，查到原来如此：
“……原来我在编译lib和exe文件的时候，一个选择Multi-threaded，一个选择Multi-threaded DLL， 最终造成了最终这样的结果。……”(引用：http://www.panzhishi.com/classyk/article.asp?id=4)

后来我用PE Explorer分别查看Multi-threaded和Multi-threaded DLL这两个选项生成的可执行文件，发现
Dependency有区别：
Multi-threaded DLL版本比Multi-threaded版本要多两个dll：msvcp80.dll msvcr80.dll，搜索后发现在WindowsWinSxS目录下，应该是vc crt的运行库。在其他电脑上没有这两个库，所以会出现这样的问题。

经过尝试解决办法有：
* 重新编译应用程序，保证所有的库和编译的程序选择Multi-threaded。
* 安装.Net Framework 2.0

下面是我的弯管机的3D演示。［已经失效］

![](images/flash.gif)Flash动画
[![](images/mm_snd.gif)在线播放](javascript:MediaShow('swf','temp39820','attachments/month_0702/SimulationNode.swf','400','300'))
