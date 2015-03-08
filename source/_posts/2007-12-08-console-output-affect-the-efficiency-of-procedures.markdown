---
author: hesicong
comments: true
date: 2007-12-08 02:00:27+00:00
layout: post
slug: console-output-affect-the-efficiency-of-procedures
title: Console输出影响程序效率
wordpress_id: 281
categories:
- 其他
---

做弯管机的仿真的工程的时候感觉速度很慢，后来发现原来是过多的控制台的输出导致。控制台的输出往往作为我调试程序的一种方案，很便捷，需要的时候可以直接输出至文件，而不像GUI的输出很麻烦。但过多的控制台输出就造成了性能的问题，在C++项目中可以通过将项目属性的“配置属性”-》链接器-》系统-》子系统，从“控制台(/SUBSYSTEM:CONSOLE)”修改为“Windows (/SUBSYSTEM:WINDOWS)”即可，但也失去了控制台本来的方便性。为了兼得调试的方便和运行的流畅，可以定义几种不同程度的输出，但这样的话增加了编程的工作量，总体来说也是值得的。

现在继续弄弯管机仿真程序啦，目前遇到的最头疼的问题还是VC++2005的C++/CLI编译太慢了，又要消耗大量的内存，而且不能利用多核的优势。还是VC++2008好一些，但问题在于有很多库有冲突，所以还只能忍一忍了。
