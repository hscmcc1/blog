---
author: hesicong
comments: true
date: 2014-12-31 15:23:33+00:00
layout: post
slug: shadowsocks-is-better-on-digitalocean
title: shadowsocks更适合搭建在DigitalOcean上
wordpress_id: 3468
categories:
- 软件
---

科学上网这个活，得要靠点技术。从最GoAgent、独立VPN，到SSH翻墙，现在用上好用好快的shadowsocks。但公用的shadowsocks服务器可能涉及到安全问题，谁知道后面有没有人持续的抓包呢？所以还是自己搭建一个靠谱。

我用的是成都的电信的光网，所以下面的经历可能和其他的网络有差别。之前选择用了[Linode](https://www.linode.com/?r=48aca8b9e959c5e0064ed57512724ca84586ca6e)，优点是内存比较大，速度也比较快，有日本的线路，应该比较适合翻墙。但用过一段时间以后你会发现，其实无论哪里的Linode，搭建shadowsocks翻墙的速度还是可以的，http浏览完全不成问题，网页载入也很快。但持续下载的时候，你会发现刚开始几秒钟，可能有上百k的速度，但是过一段时间就掉了下来，到大概10kb左右。甚至后面就干脆停掉了。这样的速度，即便是用迅雷这种多线程的工具也只能跑上几十kb。我猜测，要么是Linode自身做了QOS，要么是伟大的墙做了手脚。

一般来说shadowsocks隐藏了一些特征，我一个人用基本上墙也不care我。改端口一般会解决墙的问题，随后试过808. 44. 2. 22等端口，一样的情况。好吧，莫非是机房的原因？换到Linode的美国节点，速度一样的慢。无语了。

好吧，那看看[DigitalOcean](https://m.do.co/c/4bc532e3ef94)。

其实一年多前我就在用DigitalOcean，原因是5美元的价格非常的便宜，但是512的内存就比较慢了，优化起来也比较吃力。开了1G的swap，幸亏是SSD，还能撑一段时间。我就重新在DigitalOcean上，申请了SFO的机房，用docker的shadowsocks的images，分分钟装完shadowsocks，一试，吓我一大跳。

虽然SFO的ping比日本的ping要高一些，但整个翻墙速度没有明显的区别，youtube的1080P的也能流畅的播放。看流量能够知道shadowsocks能够持续的以400多k的速度下载，甚至有些时候能够跑满10M的带宽。我又试了用sockets代理来下载战地4的最新更新，速度也非常满意，几乎80%的速度都是1.2M/s的下载，说明shadowsocks并不是被墙了，而是Linode做了限制。

所以，对于我的电信线路，DigitalOcean的SFO能够提供非常好的质量，非常推荐大家也试用一下。更好的话，请大家用我的一个[referal链接](https://m.do.co/c/4bc532e3ef94)，这样大家都能得到一些额外的优惠。
