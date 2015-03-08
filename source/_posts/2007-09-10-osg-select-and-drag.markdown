---
author: hesicong
comments: true
date: 2007-09-10 10:51:56+00:00
layout: post
slug: osg-select-and-drag
title: OSG——选取和拖拽
wordpress_id: 243
categories:
- 其他
tags:
- opengl
- OpenSceneGraph
---


物体的选取和拖拽算是GUI里面用得比较多的部分，特别是三维的选取和拖拽更是比较麻烦。最近钻研了一下，参考了osgManipulator的拖拽部分和osgPick的选取部分，实现了选取和拖拽。程序使用的方法不一定是最好的。给大家分享分享。如果大家有更好的例子，不妨拿出来一起讨论，谢谢：)

注：VS2005工程，请自行更改头文件、库文件地址和源代码中示例模型所在地。

![下载文件](images/download.gif) [点击下载此文件](http://www.hesicong.net/blog/upload/month_0709/i2007910185153.rar)

