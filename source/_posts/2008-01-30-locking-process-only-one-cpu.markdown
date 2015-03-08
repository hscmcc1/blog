---
author: hesicong
comments: true
date: 2008-01-30 03:50:11+00:00
layout: post
slug: locking-process-only-one-cpu
title: 锁定进程只用一个CPU
wordpress_id: 303
categories:
- 其他
tags:
- .net
---

昨天调试OSG程序的时候很郁闷啊，好像是OSG有一些BUG，我那个程序如果在多个CPU上运行的话就会出现一些莫名其妙的问题。我想可能是线程争用的问题。我在任务管理器里面将程序设置为只使用一个CPU，问题就解决了，很奇怪。现在还没有时间找什么原因引起的，而每次调试都要去设置CPU关系很麻烦。只好写了一个小程序，将进程锁定在一个CPU上面，等有空了再来看到底怎么回事。

整个程序原理很简单，得益于.NET框架提供的API包装。我刚开始还以为.NET没有提供进程的CPU使用控制呢，后来搜索了一下发现竟然有：System.Diagnostics.Process.ProcessorAffinity就用于设置CPU关系的。

我设计了一个小小的界面，加了一个Timer用于随时检查是否出现了进程，出现了就自动锁住。很方便。

![](/images/Word/013008_0350_CPU1.png)

下面是源代码和可执行文件，请用VS2005打开。PS：其实也就那么两三行……

[[download id="8"]
](http://www.hesicong.net/blog/upload/2008/1/CPUControl.rar)
