---
author: hesicong
comments: true
date: 2008-10-15 15:17:10+00:00
layout: post
slug: place-in-the-ntfs-partition-under-linux-virtual-machine-file-please-note
title: Linux下在NTFS分区放置虚拟机文件请注意
wordpress_id: 383
categories:
- 软件
tags:
- Linux
---

今天发现Vmware 6.5正式版在ubuntu下面运行非常缓慢，ntfs-3g cpu占用竟然达到了25%(四核，单核就100%)以至于根本无法使用虚拟机，会卡的要死。后来网上查了一下，在虚拟机的.vmx文件里面加入下面一句就可以解决：

```
mainMem.useNamedFile = "False"
```