---
author: hesicong
comments: true
date: 2005-08-08 09:37:58+00:00
layout: post
slug: improve-debugging-net-cf-procedures-are-some-tips
title: 提高调试.net cf程序效率一些技巧
wordpress_id: 1366
categories:
- 其他
tags:
- .net
---


最近在写PPC程序，反复的调试程序以后总结了一些提高调试效率的方法。
* 首先最好要有一个真实的PDA。模拟器的运行速度比真实的PDA速度慢很多。
* 推荐PDA Controller Professional。用于远程控制PDA，放在桌面上并设置为PDA一连接就启动。可以实时的看到PDA上的画面并提供软启动、任务管理，文件管 理，屏幕截图，操作录像等功能。这就不用每次都去看、去点PDA屏幕，而且输入直接用键盘，挺快的。软启动功能就不用说了吧，经常遇到共享冲突的时候就会 起到很大的作用。

下面是他的一个截图：
![](http://images.cnblogs.com/cnblogs_com/hesicong/ppc%E8%B0%83%E8%AF%95.JPG)

另外我在PDA上还安装了Battery Pack Pro这个软件，方便关闭程序。
![](http://images.cnblogs.com/cnblogs_com/hesicong/ppc%E8%B0%83%E8%AF%952.JPG)
. 安装.NET CF SP3。安装后发现程序启动和调试速度有明显的提高。
.  如果有VS2005的话可以用它写程序。更好的IDE和更完美的提示可以免除很多烦恼。例如有些时候Dim tmp as OneClass忘了加new关键字，而在后来又使用了tmp.Method()，可就麻烦了，调试器会提示出错，很多时候强行中止程序会导致共享冲突， 无法继续调试。这时候只好软启动。而VS2005里面如果没有初始化的话会给出提示。
. ……继续总结中……先就写到这儿。
