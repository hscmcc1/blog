---
author: hesicong
comments: true
date: 2008-05-03 07:51:09+00:00
layout: post
slug: gpu-voxelization-of
title: GPU体素化初探
wordpress_id: 354
categories:
- 其他
tags:
- opengl
- OSG
---

用OSG实现，`256*256*256`的分辨率，每一帧都对物体进行体素化并用简单的体渲染进行显示，可以达到20FPS以上的速度，基本满足我的需要了。但我用的体素化的方法还存在一些问题，对于一些特殊的形体还无法正确体素化。 注：只有NVIDIA 8系列显卡以上才支持硬件渲染到3D贴图。

效果：

[](/images/others/volumetric.jpg)![](/images/others/image/thumb/volumetric.jpg)

对这个物体进行体素化：

[](/images/others/3dsmax.jpg)![](/images/others/image/thumb/3dsmax.jpg)
