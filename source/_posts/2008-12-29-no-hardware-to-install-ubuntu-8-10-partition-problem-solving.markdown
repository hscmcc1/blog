---
author: hesicong
comments: true
date: 2008-12-29 02:08:31+00:00
layout: post
slug: no-hardware-to-install-ubuntu-8-10-partition-problem-solving
title: Ubuntu 8.10硬件安装无分区问题解决
wordpress_id: 1509
categories:
- 软件
tags:
- Linux
- ubuntu
---

可能是安装的时候占用了硬盘资源，导致无法找到硬盘分区，结果安装就没法正常的分区，安装不能正常进行。最后，参考了[这个帖子](http://www.javaeye.com/topic/293639)，只需要在isodevice umount掉就可以了。直接打开“Application"->"Accessories"->"Terminal"，然后

```
cd /
sudo umount -l isodevice</blockquote>
```

问题解决。笔记本升级为8.10，HAPPY。

PS：Virtual Box 2.1的性能确实要比臃肿的Vmware 6.5快很多，而且也没有Vmware 6.5的键位映射问题。爽～！
