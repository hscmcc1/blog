---
author: hesicong
comments: true
date: 2007-02-27 04:58:40+00:00
layout: post
slug: learning-openscenegraph
title: 学习OpenSceneGraph
wordpress_id: 141
categories:
- 其他
tags:
- OpenSceneGraph
- OSG
---

OpenSceneGraph是基于OpenGL的跨平台的开源的SceneGraph。和OpenSG的区别见[这里](http://opensg.vrsource.org/trac/wiki/OSGComparison)
SceneGraph就是场景图，在学习计算机图形学的时候提到过这个名词，隐约的感觉就是一个层次式的结构。后来在做弯管机的虚拟现实的时候就用到了层次式的结构，也用到了Node等概念，其实也就是场景图的简单应用而已，只用于变换各个部件和层次式的渲染。

前些天看了Avi Bar-Zeev的[Scenegraphs: Past, Present and Future](http://www.realityprime.com/scenegraph.php)，知道了场景图的各种用途－－渲染前剔除、层次结构渲染、碰撞检测等等。

为了体验其强大的功能，今天写了一个小小的几行验证了一下：

```
osg::Node* testNode=NULL;
testNode=osgDB::readNodeFile("F:/machine.3DS");
```

哇，顺利读出文件！

后来经过测试完美支持3DS文件，这下爽坏了！！3DS文件里模型的层次式结构也可以很方便的调用出来，这下弯管机的虚拟现实就能做得更好了！

高兴高兴……!

一个遗留问题，和MFC的交流如何……
