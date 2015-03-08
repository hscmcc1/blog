---
author: hesicong
comments: true
date: 2007-07-02 23:00:36+00:00
layout: post
slug: resolve-e50-media-failure-can-not-be-updated
title: 解决E50媒体无法更新的故障
wordpress_id: 214
categories:
- 其他
tags:
- Phone
---

刚入手的E50竟然添加了MP3无法扫描到，这是成何体统？在网上搜索了一下也没有发现解决方案，只好自己摸索一下。

其实问题很简单，卡里面文件系统有问题。用读卡器读卡，在命令行下面输入“CHKDSK 盘符 /F”修复即可。

检查出一个问题，在OthersContact文件夹下面有非法长文件，原来是这里作怪。将卡插回手机，故障排除……

智能系统真是奇妙……
