---
author: hesicong
comments: true
date: 2008-06-05 22:49:18+00:00
layout: post
slug: cuda-intelisence
title: CUDA Intelisence
wordpress_id: 361
categories:
- 其他
tags:
- CUDA
---

CUDA是个好玩意儿，现在2.0BETA已经出来了，提供了很多特爱的特性，特别是对于3D纹理的支持。我也不清楚CUDA应用在目前我的一些项目中到底有多少潜力，但看来这个还是个趋势，可以了解一下。今天在配置CUDA编程环境的时候，因为其后缀名是.CU的，所以VS2005的智能感知无法使用。再查询了一些文章以后，发现有下面的解决方案：

We can use the tool Visual Assist X to implement it:
First, find the Visual Assist X install directory:
Program FilesVisual Assist XAutotext, then make a copy of Cpp.tpl, rename it to Cu.tpl, apply same operation to the "Latest" directory.
Second, open the regedit table, and search it with the key word "Visual Assist X" until you find in the VANet8 in HKEY_USERSS-1-5-21-1757981266-220523388-725345543-1003SoftwareWhole TomatoVisual Assist XVANet8
Here you can see many attribute setting about Visual Assist X,
click the item ExtHeader and add the .cu in the list, same to the item ExtSource.
save and quit.

[原文](http://forums.nvidia.com/index.php?showtopic=35669&hl=intellisense)
