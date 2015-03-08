---
author: hesicong
comments: true
date: 2005-02-09 15:26:33+00:00
layout: post
slug: net-ide-interface-programming-autoscale-property-mess-in-big-trouble
title: .net IDE 界面编程AutoScale属性惹的大麻烦
wordpress_id: 1188
categories:
- 其他
tags:
- .net
---


最近我我在英文XP SP2环境下制作了一个比较精美的界面，请一个同学帮忙测试。他用的是中文版的操作系统。然而奇怪的是界面大小发生了改变，在中文环境下窗体变大了，按钮 也变大了，所有的Label也移位了。我开始以为是他那里Windows设置的问题，后来在我新装的一个中文环境当中也出现了同样的问题，很是纳闷。

后来到处寻找原因，以为是微软的一个BUG。后在CSDN里面找到了答案，竟然是小小的AutoScale属性惹得货。
AutoScale属性默认设置为True，也就是根据字体的大小自动缩放窗体。很有可能在英文XP环境下的字体和中文环境里面的不一样(但看起来是一样的)，导致了这个问题。最终把AutoScale属性设置为False，再编译，一切问题都解决了。

所以得到一个小经验，在需要开发多语言的程序的时候一定要把AutoScale属性设置为False，不然很好的界面(特别是图形化的)就会变得面目全非。另外使用Dock，Anchor也对界面维护起到一定的效果。
