---
author: hesicong
comments: true
date: 2007-02-10 07:29:35+00:00
layout: post
slug: 5200-miraculously-revived
title: 5200奇迹般死灰复燃！
wordpress_id: 134
categories:
- 硬件
---


躺了3年的丽台FX5200终于重获天日。说是奇怪那真的是奇怪，就貌似木乃伊复活一样神奇。

这张显卡是我高中毕业的时候花了600多买的显卡，当时还算是搭上了DX9的列车。可好戏不长，搬家那天我顺便打扫一下电脑，居然显卡就不能用了。这个显卡设计得比较好，卡上面有三个灯，分别对应ERR、PWON和AGP4X/8X，可以很直观的看到显卡的工作状态。然而，一开机，就是红灯，ERR。主板上显示1D，我也不知道具体是什么问题，只是打电话到客服，客服告诉我显卡出问题了。唉…………~修一下看能不能修好。

然后就拿到电脑城去修，结果插到别人电脑上也是红灯，显卡核心没有温度，断定为核心烧毁。当时想想也有可能，静电嘛。

然后就买了昂达的5700，5200就躺在了柜子里面。

今天想用一下改装过的5700测试程序的问题，结果5700插上主板也是报告1D错误，妈啊，不会这个5700也烧了吧。前不久我才用过的啊！那我那个5200插上呢？还是这样，红……拿出那个老古董TNT2，可以启动。

那就到那个120块钱的电脑上面去看看5700到底是好是坏，奇怪，一点就亮。那这个5200呢？插上去，我以为要“红”，结果一“绿”，启动成功~!!!!!咋个回事呢…………!!!我又把5200插回磐正的主板上，奇迹般现在不“红”了！基本工作正常！郁闷哦，之前这个5200我还想既然坏了，那就拆零件当焊板玩的，结果，竟然是一个好的~!!!!!!!!!!!

把6600GT装上，成功启动，网上查了查，原来是说Initial EARLY_PM_INIT switch，具体也不知道是什么开关怎么了。磐正8RDA型号很多主板都遇到了这样的问题，然后就是显卡不能用了。真是奇怪。

有些人说是和IDE冲突什么的，我就把把CMOS复位了，关闭所有能够关的，取下6600GT然后插上5700，还是一样的1D！！！

我分析，6600GT因为在没有插外接电源的情况下是2D工作，所以耗能很少，主板AGP供电能够启动。TNT2也是如此。但是520. 5700启动时耗能较多，可能是AGP供电部分出了问题(网上提到很可能是电解电容电容失效了)而不能启动。原因基本上想通了，就在BIOS里面把AGP电压从1.5V提升到1.6V，再把5700换上去，结果成功启动。但5200还是无法启动，难道1.6V还不够。为了安全，我也没有再提升电压，只要有5700用就足够了。

好累，搞得一手都是灰…………~还不知道我这个主板能够坚持好久哦…………睡觉…………~
