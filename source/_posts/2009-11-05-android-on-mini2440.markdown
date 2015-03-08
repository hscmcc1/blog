---
author: hesicong
comments: true
date: 2009-11-05 14:47:19+00:00
layout: post
slug: android-on-mini2440
title: Android on MINI2440
wordpress_id: 2078
categories:
- 硬件
tags:
- android
- mini2440
- s3c2440
---

我今天把网友编译好的Android修改好，主要是修改8寸屏幕的定义部分而已，放到我的MINI2440上运行了一下，能显示菜单能动作，就是速度那简直是太慢了，应该是和8寸屏幕有关系，或许三寸屏幕要好一些。没办法玩了。

最近试了试基于Linux的GUI，结论如下：XFCE相当慢无法用；GNOME Mobile也很慢；Enlightenment还凑合，只是运行其他程序就慢很多了；Android在我的八寸屏幕上无法用，截图我就不发了。

现在就只有耐心的等待友善出WINCE6的BSP了，否则只好弄个WINCE5的系统来玩玩。Linux恐怕还是最好玩玩控制台算了。呵呵。

当然，主要是我的目标太高了，想做个类似于MID的系统，但64M的RAM和400M的CPU，恐怕是有点遗憾呢。
