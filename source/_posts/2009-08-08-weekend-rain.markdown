---
author: hesicong
comments: true
date: 2009-08-08 14:53:24+00:00
layout: post
slug: weekend-rain
title: 周末下雨
wordpress_id: 1906
categories:
- 其他
tags:
- 双楠
- 美联
---

刚刚热了一阵又开始下雨了，天气又变得凉爽起来。不经意间已经立秋了，苏州的小妹都在卖月饼了，这年头，卖月饼也要提前两个月。

这周过的很快，一下就到了周末了。周四去调了铣床功能感觉还不错，原本以为比较复杂的问题最后还是开窍了，解决了。其实整个数控项目中最深恶痛绝性能问题都来自于GDI+的性能，缺少GDI的硬件加速，没有提供XOR画笔是最大的一个软肋，直接写屏幕，只能搞迂回战术。最后最让头奇怪的是竟然没有提供画点的函数，还非得用画一个1X1的矩形来解决，莫非当年的.NET 2.0设计者也不知道封装么？~

当然，最后问题的解决还算是顺利，使用了离屏缓存加直接写屏解决了直接写屏和双缓冲绘制的难点，效率也得以提高，算是one stone two birds~

本来打算的15号至22号出去旅游的，由于种种原因取消了，弄得心情也不好。今年天灾人祸也多，台风暴雨泥石流，去哪儿都不安全啊，还是呆在家里面算了。

下午第一次去了美联的双楠校区，很大，人比较少，上课没有八宝街那边拥挤，但多是beginner，有些话题不太好聊，都保持一定的沉默呵呵。在双楠这儿找到个比较稳定也比较快的无线接入点，要是有个笔记本就好了。唯一不好的就是周边没有什么吃的，仅找到一家快餐店，晚上吃了个青椒肉丝炒饭，6块钱，没有吃饱……

其实从双楠校区到我家挺近的，开车也就10多分钟就到了，晚上坐公交车走的，一折腾折腾了一个小时。可怜的78路啊，老是要等很久，然后突然来了两趟……光在金沙等78路久等了二十多分钟，以后还是开车或者骑车好了，方便快捷。

下周还是继续调机床了~争取在9月份之前有个了结比较好。



