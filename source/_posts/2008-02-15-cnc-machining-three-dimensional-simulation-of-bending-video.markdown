---
author: hesicong
comments: true
date: 2008-02-15 05:45:21+00:00
layout: post
slug: cnc-machining-three-dimensional-simulation-of-bending-video
title: 数控弯管机加工三维仿真(视频)
wordpress_id: 318
categories:
- 其他
tags:
- opengl
- OSG
---

这是我一直在做的数控弯管机加工三维仿真程序的视频。目前整个程序用C++/CLI写成，界面用的是.NET的控件(颜色因为要和之前工程保持一致，所以比较丑)，三维部分使用OSG进行显示，控制使用托管C++进行编程以便和OSG进行交互。三维机床使用Solidworks建模然后进行处理导出为OSG支持的格式。夹具使用实体布尔运算生成，可进行参数化生成。工艺流程由另外小组的工程生成，然后转换成本程序能够识别的格式进行动作模拟。本程序大量应用C++/CLI技术，将.NET平台的优势进行了最大程度的发挥，以后的工作会对程序进行更多的BUG除错和优化。

(2008-11-27：由于老主机不支持视频播放，该链接已经失效)
