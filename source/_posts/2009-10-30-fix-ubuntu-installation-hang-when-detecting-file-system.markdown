---
author: hesicong
comments: true
date: 2009-10-30 14:58:36+00:00
layout: post
slug: fix-ubuntu-installation-hang-when-detecting-file-system
title: Fix Ubuntu installation hang when detecting file system
wordpress_id: 2069
categories:
- 软件
tags:
- hang
- Linux
- ntfsresize
---

My Ubuntu hanged every time when detecting file system. Sometimes even I can't see the GUI of Kubuntu. I found out that when it hanged my hard disk was reading something. I encountered this problem several years ago, but the detecting process ended not very long after the "Detecting file system" dialog. This time, after 10 minutes waiting, no response. I had to power off my PC.

Last time I thought the problem was caused by non standard or problematical partition. I remembered that I had one logical partition between two primary partitions. I fixed this problem and the installation went on.

But the problem was not the same as my last time, I fixed had a regular partition. I found out that I had some disk errors so I "chkdsk" first my drivers. No luck. I had no ideas about this. I tried to search the problem but no one had the same issue I met. So I had to figure it out by myself.

I tried to unplug the disk first and then after the liveCD desktop showed I plug it so I can see the GUI interface. Just after the "Detecting file system", the hard disk was still reading something without a stop. I switched to another console and type "top" to find the most CPU consume program. One of them is "ntfsresize". So I killed this program. But it appeared again. I killed it until no new "ntfsresize" was shown. And finally I saw the partition. Then the whole installation went on smoothly.

So, this problem must be caused by the "ntfsresize". I didn't know what's wrong with my disk or partitions that "ntfsresize" need to read it again and again. I even did not need to resize my ntfs partition. Next time, kill it first~
