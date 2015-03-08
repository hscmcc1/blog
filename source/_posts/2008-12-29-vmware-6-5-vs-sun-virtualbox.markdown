---
author: hesicong
comments: true
date: 2008-12-29 14:20:31+00:00
layout: post
slug: vmware-6-5-vs-sun-virtualbox
title: VMWare 6.5 vs Sun VirtualBox
wordpress_id: 1512
categories:
- 软件
tags:
- Linux
- virtualbox
- vmware
---

昨天我还以为找到了福音，结果今天工作了半天才发现，最新的VirtualBox 2.1的性能还是有问题。昨天看到，XP启动的时候VirtualBox的性能确实好与VMWare 6.5，在实际使用中也发现VirtualBox的磁盘性能也比VMware6.5好。莫非是我虚拟机里面选磁盘接口的时候VirtualBox选的SATA，VMWare选的IDE造成的？

最大的性能差异在于应用程序的执行速度。平时我要开一个VS2005+Resharper插件，VMWare6.5分配了两个CPU给他用，很好用，速度很快。但VirtualBox就恼火了，只能开一个CPU不说，而且核心占用率经常是和实际CPU占用率一样，天啊。Resharper平时本来就要不停地解析，CPU占用又经常是100%，连打字都是飞着打的。郁闷。坚持了两个小时以后，终于放弃VirtualBox了。

总结起来，VirtualBox确实还需要进步阿！期待小巧实用的VirtualBox进一步发展，打败庞大的VMWare。
