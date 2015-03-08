---
author: hesicong
comments: true
date: 2013-01-19 06:51:15+00:00
layout: post
slug: bai_du_yun_rom_adb_shen_qi_de_wen_ti
title: 百度云ROM adb神奇的问题
wordpress_id: 3208
categories:
- 生活
---

华为G330D在装了百度云ROM后，在Ubuntu 12.04下，用最新的google adt，发现无法显示设备，一直出现问号。

```
root@derek-desktop:/home/derek/adt-bundle-linux-x86_64/sdk/platform-tools# ./adb devices
List of devices attached
????????????    device
```

类似这种问题，之前遇到过是显示no permission。使用sudo ./adb kill-server然后再用root启动adb即可。

但这次的问题不一样了，似乎是序列号没法显示。后来在Windows系统上，也基本上是同样的问题。

我记得之前用华为官方的系统是没问题的，所以肯定是百度云ROM有问题。在Google了很久以后，没有任何解决方案，差点就放弃了。

后来看了一下adb的使用，发现有一个adb root命令。试试吧。

```
root@derek-desktop:/home/derek/adt-bundle-linux-x86_64/sdk/platform-tools# ./adb root
restarting adbd as root
root@derek-desktop:/home/derek/adt-bundle-linux-x86_64/sdk/platform-tools# ./adb devices
List of devices attached
0C37DC6F22FC    device
```

居然可以了……

就这么神奇的解决了该问题。

推测手机上的adbd不是用的root权限运行的！

另外，还发现弹出移动USB大容量存储设备后，再切换到HOME界面，会导致ADB终端，并弹出大容量设备。真是神奇～～～
