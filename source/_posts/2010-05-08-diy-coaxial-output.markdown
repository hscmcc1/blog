---
author: hesicong
comments: true
date: 2010-05-08 13:49:42+00:00
layout: post
slug: diy-coaxial-output
title: 同轴输出DIY
wordpress_id: 2260
categories:
- 硬件
tags:
- alc888
- diy
- rca
- realtek
- spdout
---

以前，我的nForce2的老主板有同轴、光纤输出，只是还未等到用上它的时候就已经挂了。这挂也挂的挺神奇的，听着音乐，突然就死机了，然后音响里面传来几声噪音，随后闻到一股糊味。再开机，已经无法开机了。打开电脑机箱一看，声卡自己糊了。能不能拆了声卡用呢？把声卡的芯片取了，无法开机，主板报废。

现今有了天逸的AD9600功放，DVD机到功放都是同轴输出的，电脑能不能行呢？

一查主板的说明书，发现主板上有SPDOUT接口。可以接光纤或者同轴。我的微星主板上有三个脚，分别是GND,VCC,DATA。只需要连接GND,DATA就可以有输出了。

搞懂了原理，在电子市场买了一个同轴的插座，找了以前老光驱上的线，接到SPDOUT输出上，一切搞定。搞定之后，拆了一个旧声卡的挡板，把上面的洞扩大，将插座固定到机箱上。

有了同轴输出，当然还得要有音源，立刻演了一下最新下载的720P的阿凡达，讲的是暴力拆迁的故事。音质相当令人感动，环绕音响能够听到2声道听不到的细腻声音。

然后又放了蓝光的《叶问》，再次被其震撼的音效和精彩的打斗场景折服！

最后试了试BFBC2(战地：叛逆团队2)，遗憾的是，虽然开始的时候有Dobly Digital的标示，但是游戏里面无论怎么都无法得到数字输出的效果。我想估计只有少数的声卡支持，我等的ALC888声卡就算了。呵呵。

目前看来，DIY算是成功了，成本2.5元，音质不错，家庭影院系统更上一个台阶~
