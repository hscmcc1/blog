---
author: hesicong
comments: true
date: 2008-08-08 20:24:20+00:00
layout: post
slug: gpu-texture-fill-rate-test
title: 测试GPU的材质填充率
wordpress_id: 371
categories:
- 其他
tags:
- Opengl OSG
---

体渲染最重要的一个优化就是减少GPU的采样工作。测试GPU的材质填充率能够指导我们的工作。要知道为什么GPU在800*600的环境中只能达到12FPS么？这就要看GPU每秒钟采样的次数啦。

我写了一个简单的ＯＳＧ程序，用来测试采样次数：[download id="3"]

程序原理很简单，分几步：创建窗口->生成和设置纹理->载入SHADER->渲染。具体如何做要看程序里面啦，这里就不再贴出来了。

直接说最后的测试的结果了。我的8800GTS(G80)官方资料说材质填充率能够达到24Billion/Sec，官方资料给的核心频率500Mhz，着色器频率1200Mhz，显存800Mhz。我将我的显卡也按照这个数据进行了降频。

测试环境：窗口800*600，3D贴图256*256*256，数据是LUMINACE_ALPHA，每个像素2BYTE。每个像素的Shader采样3D贴图512次。

最后得到测试FPS为11.98帧。算算：800*600*512*11.98=2,944,204,800，因为是3D纹理所以每个采样实际要有8次采样工作，所以最终的材质填充率：23,553,638,400，和24Billion/Sec很接近了。

换用2D贴图可以得到相似的结果，只是FPS会快一倍。原因是三线性采样的工作量是二线性采样的两倍，很显然FPS会提升一倍。

那么怎么去优化呢？下面做一些测试：
1. 减小3D贴图的大小，以便尽可能的装到CACHE里面。直接设置为1*1*1，结果发现性能一样。
2. 更换3D贴图的internal format为RGBA，发现性能一样。
3. 超频：超核心，性能提升百分比和超频百分比几乎一样。超Shader，几乎没变化。超显存，几乎没变化。
4. 降频：降核心，性能降低百分比和降频百分比几乎一样。降Shader，几乎没办法。降显存，一直降到400Mhz也没有变化。
5. 降低每像素采样率：降低为256后，性能提升一倍，和预期一样。
最终的结果很明显。3D贴图的采样已经成了整个系统的瓶颈，已经让显卡的贴图单元达到了极限。Shader处理器由于计算量很小，所以还很空闲。由于采样过滤的繁忙，贴图单元也不需要很大的显存带宽，所以显存的影响几乎没有。

优化的措施：只能尽可能的减少采样次数，或者找更快的卡。目前看来只有G92的9800GTX或者8800GTS的采样率能够达到43.2Billion/Sec以上，GTX280官方资料也只能达到48.2Billion/Sec，GTX260 36.9Billion/Sec。9800GX2能够达到76.8Billion/Sec，就是不知道实际SLI的性能能不能满足需要了。看来如何选择适合体渲染的卡已经有一个理论和实际的指导了。
