---
author: hesicong
comments: true
date: 2007-04-04 02:54:09+00:00
layout: post
slug: openscenegraph-notes-the-world-is-so-beautiful
title: OpenSceneGraph 笔记--世界如此之美好！
wordpress_id: 175
categories:
- 其他
tags:
- OpenSceneGraph
- OSG
---


在用OpenSceneGraph之前我费尽心思成功的把3DS模型里面的层次关系导入到我的程序中，学到了不少的东西。昨天在研究OpenSceneGraph的3DS插件的时候发现插件并没有我想象当中那么完美，在调入文件的时候丢失了所有的层次结构，这令我很懊恼。在尝试了修改3DS插件以后，觉得要解决这个问题还需要重新写一个新的插件。当然，这会带来巨大的工作量，不合算。

今天在吃饭的时候突然想到了OpenSceneGraph自己有一个数据结构，扩展名叫.osg，而且有3DS MAX的导出插件，是不是这个数据结构能够提供更多的信息呢？

带着试一试的心理下载了OSGExp插件，晕，第一次安装竟然不行，提示找不到3DS MAX～～想想我装的3DS MAX 9是中文版的，是不是要原版的才能用哦。遂安装了英文原版的。呵呵，这下就对了。

从3DS MAX里面打开做的机床的MAX模型文件，然后导出，很快的就导出了一个osg文件。用osgDirector一看，哇！强悍，所有的层次都保留，层与层之间的变换矩阵也很好用！特别是之前很懊恼的3DS模型里面的pivot属性也越过了。

最后我在自己的程序里面试了试，很方便的就能够让某些部件绕着轴旋转或者做平移变换，而代码却只有十多行！

想起之前我自己动手写的一个很简单的SceneGraph实现，都花费了我很多精力，处理用lib3ds导入的模型的矩阵变换。然而现在OpenSceneGraph让一切变得很美好，这下就有精力专注于上层程序开发了！

也肯定之前的工作没有白费，他们让我熟悉了矩阵的变换以及一个SceneGraph的原理，这些都为现在学习OpenSceneGraph奠定了基础！OpenSceneGraph带给我的只是更多更强大的功能，还有很长的路要走！

另：所有可以导出为osg格式的文件都最好导出成osg，这样会更有利于开发！
