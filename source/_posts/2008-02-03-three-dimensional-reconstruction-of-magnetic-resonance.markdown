---
author: hesicong
comments: true
date: 2008-02-03 09:11:45+00:00
layout: post
slug: three-dimensional-reconstruction-of-magnetic-resonance
title: 核磁共振三维重建
wordpress_id: 307
categories:
- 其他
tags:
- opengl
- OSG
---

哈哈，看看我做的核磁共振图像重建的渲染，多亏我的8800GTS了，要不然还无法做得出来。目前还处在开发阶段，可实现实时任意切割，速度还可以优化，主要是Shader的书写和一些压缩算法的考虑等等，还可能要使用一些更高级的特性来加速。800*600窗口256采样率的时候从35-50FPS，512采样率就降到了25-30FPS，打开半透明后性能下降比较严重，拉近后在10FPS左右，基本达到实时性要求，关闭一个灯光的光照后性能提升会更明显。下图是今天重新调整着色后的样子，色彩不遵循真实色彩(很难调，需要细心的调整，目前用的是1D传递函数，2D的更难调)，好看就可以：)
图：渲染结果
[](/images/others/1_0.jpg)![](/images/others/image/thumb/1_0.jpg)

图：进行实时切割
[](/images/others/2.jpg)![](/images/others/image/thumb/2.jpg)

图：减少透明度
[](/images/others/3.jpg)![](/images/others/image/thumb/3.jpg)

图：减少透明度，减少可渲染的物质
[](/images/others/4.jpg)![](/images/others/image/thumb/4.jpg)

图：半透明渲染1，2
[[](/images/others/5_0.jpg)![](/images/others/image/thumb/5.jpg)[](/images/others/5_0.jpg)](/images/others/5.jpg)
[](/images/others/6.jpg)![](/images/others/image/thumb/6.jpg)

图：图片阵列。显摆一下我的双显示器，双1280*1024分辨率，哈哈

[](/images/others/7.jpg)![](/images/others/image/thumb/7.jpg)

图：1D传递函数设计器，C#做的，实时调整颜色和透明度，有图片预览。设计器还有一些BUG，呵呵。

[](/images/others/8.jpg)![](/images/others/image/thumb/8.jpg)
