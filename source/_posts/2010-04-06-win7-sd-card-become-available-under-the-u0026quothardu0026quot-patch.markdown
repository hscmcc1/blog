---
author: hesicong
comments: true
date: 2010-04-06 14:54:50+00:00
layout: post
slug: win7-sd-card-become-available-under-the-u0026quothardu0026quot-patch
title: Win7下可用的SD卡变“硬盘”的补丁
wordpress_id: 2212
categories:
- 硬件
tags:
- SD
- SDHC
---

SD卡在WIN7里面通过磁盘管理仅能分成一个分区，包括Acronis Disk Suite和Paragon Disk Manager再内的软件都不能正确的分区。只有把SD卡从“可移动磁盘”变成“硬盘”类型，才能进行分区。前段时间找了一下，有一个补丁竟然把Win7整蓝屏了。今天又找了一个，很好用，见附件。

用法是把SD卡的驱动换成这个驱动就OK。在“设备管理”中，找到“SD”卡，然后更换为这个驱动(cfadisk.inf)
