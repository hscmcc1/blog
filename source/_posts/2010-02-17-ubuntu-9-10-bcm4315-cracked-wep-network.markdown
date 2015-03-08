---
author: hesicong
comments: true
date: 2010-02-17 15:50:21+00:00
layout: post
slug: ubuntu-9-10-bcm4315-cracked-wep-network
title: Ubuntu 9.10+BCM4315成功破解WEP网络
wordpress_id: 2187
categories:
- 硬件
- 软件
tags:
- '4315'
- bcm4315
- bcm43xx
- spoonwap
- ubuntu
- 蹭网
---

我的联想U150的本本的BCM4315网卡一直没有得到BT4和BT3的完美支持，就连Ubuntu 9.10装好后也需要自己再装Restricted Driver。这个驱动装好后好像不能被aircrack系列支持。我认为只要把4315驱动搞定了，就能在任何Linux系统下应用aircrack。那么第一步就是把b43驱动驱动起来。
寻找了很久以后找到了这个帖子：http://ubuntuforums.org/showthread.php?t=1266620
使用2.6.32.12的kernel加上b43的驱动就可以完全搞定它。
Ï32位系统的内核：
http://mirrors.kernel.org/ubuntu/pool/main/l/linux/linux-image-2.6.32-12-generic_2.6.32-12.17_i386.deb
http://mirrors.kernel.org/ubuntu/pool/main/l/linux/linux-headers-2.6.32-12-generic_2.6.32-12.17_i386.deb
http://mirrors.kernel.org/ubuntu/pool/main/l/linux/linux-headers-2.6.32-12_2.6.32-12.17_all.deb

* 安装内核：

``` bash
sudo dpkg -i linux*2.6.*.deb
```

* 下载[compat-wireless驱动](http://linuxwireless.org/download/compat-wireless-2.6/)，照着它的说明，我用最新的编辑无法成功，用compat-wireless-2010-01-26.tar.bz2即可。
* 编译：

``` bash
make
sudo make install
```

* 卸载老的驱动

```
sudo make unload
```
* 刷新驱动

```
sudo depmod
sudo depmod -a
```

* 打开PIO模式：

```
echo "options b43 pio=1" | sudo tee -a "/etc/modprobe.d/b43-thingy.conf"
```

* 屏蔽STA驱动：

```
echo "blacklist wl" | sudo tee "/etc/modprobe.d/wedontneednonossdrivers.conf"
```

* 启动时打开B43驱动：

```
echo "b43" | sudo tee -a "/etc/modules"
```
1* 打开/etc/rc.local， 在exit(0)之前加入

```
modprobe -r b43
sleep 3
modprobe b43
```

然后就可以用aircrack的工具进行破解了。
spoonwap在ubuntu的sudo环境下回提示表达式不正确，解决方法是

```
sudo ln -sf bash /bin/sh
```

然后享受吧，我用了1分多的时间把自己的无线网络破解了，隔壁的网络没有活动的客户端破解相对繁琐，改日再研究。
