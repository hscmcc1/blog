---
author: hesicong
comments: true
date: 2010-04-29 17:10:21+00:00
layout: post
slug: lenovo-u150-adjust-brightness-under-linux
title: Linux下调节Lenovo U150亮度
wordpress_id: 2232
categories:
- 软件
tags:
- brightness
- Linux
- u150
---

自从买了U150以后，集成的GMA X4500MHD的亮度就从来不能调节。最多只能在grub菜单的时候进行调节，很是头痛。在网上大力搜索了一番以后，找到了[Y450的童鞋们遇到的相同的问题](http://forums.lenovo.com/t5/Linux-Discussion/Y450-linux/td-p/130370)，这个帖子里面有一个总结并且写了[一个脚本来关联用键盘调整的亮度和实际亮度的关系](http://elektropionir.net/)，最核心的东西在于：

```
sudo setpci -s 00:02.0 F4.B="20"
```

后面的20是亮度，值从00到FF，十六进制。可以实现255级亮度的调节，还是很理想的。相对于Windows里面的级别更是多了一些。

setpci通过设置pci接口寄存器的值来进行调节，可能Windows里面强行设置pci的寄存器的值也能达到相同的效果。

运行提供的脚本以后，不管怎么说，在linux也能调节了。虽然这样做起来有点别扭。

最后，麦克风和摄像头似乎还是没法驱动。最新的Ubuntu 10.04也没有解决，看来Linux的道路上，驱动和兼容，成了最大的问题。
