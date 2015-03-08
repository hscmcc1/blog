---
author: hesicong
comments: true
date: 2007-02-04 06:10:16+00:00
layout: post
slug: collision-detection-u0026amp-machine-tool-model
title: 碰撞检测&机床模型
wordpress_id: 127
categories:
- 其他
tags:
- opengl
---


下午优化了程序流程，缓冲了一些东西，使碰撞检测的速度大大提高。现在4个57600面的运动茶壶相互进行碰撞检测(一帧6次检测)，线框模式下可以维持在20FPS左右(注：线框模式在非专业显卡上速度较慢)。成绩已经比较满意了，至少足够应用了，工控机P4 2.8+512内存已经足够了。但是麻烦的是不知道集成显卡的性能好不好。以后要测试在虚拟机下面的成绩，还有其他低配置电脑上的运行效率。

图：4个57600面的茶壶的碰撞检测。红色的地方是检测到的碰撞中的第一个三角形，太小了已经很难分辨了。
[![collision.jpg](/images/others/thumbs/thumbs_collision.jpg)](/images/others/collision.jpg)

优化完碰撞检测以后，就出去散步了。和老爸走了几公里的路，晒晒太阳，多温馨的：)

晚上回来就没有怎么弄编程了。就开始制作机床的模型，毕竟实用当中是以实际机床模型作为碰撞检测的基础的。照着一个参考机床用3DSMAX做了一个，慢慢熟悉3DSMAX的操作，还不错，做出来还像个样子。

图：我做的弯管机机床
[![machine.jpg](/images/others/thumbs/thumbs_machine.jpg)](/images/others/machine.jpg)


明天还要出去耍喔，到洛带，爬“长城”！
