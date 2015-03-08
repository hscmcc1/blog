---
author: hesicong
comments: true
date: 2008-12-25 15:18:12+00:00
layout: post
slug: python-s60-in-the-linux-development-process-under-the-initial-even-on-mobile-phones
title: 在Linux下开发Python S60程序初步——连上手机
wordpress_id: 1492
categories:
- 其他
tags:
- python
- s60
---

今天突然想起来了，以前在Windows里面可以用超级终端链接通过蓝牙连接手机，但在Linux下面怎么做呢？做个笔记：

```
sdptool add --channel=10 SP
while true; do rfcomm listen /dev/rfcomm0 10; done

# On the cell phone, Open "Bluetooth Console" in Python menu and
# choose the Linux PC as the machine to connect to
#
# Once the phone is connected, open another terminal and type

screen /dev/rfcomm0
```

输入后会出现以下字样：

```
hesicong@hesicong-desktop:~$ sdptool add --channel=10 SP
Serial Port service registered
hesicong@hesicong-desktop:~$ while true; do rfcomm listen /dev/rfcomm0 10;done
Waiting for connection on channel 10
Connection from 00:1D:FD:F0:A5:AD to /dev/rfcomm0
Press CTRL-C for hangup
```

也就打开了蓝牙的串口，然后在手机上打开Python的蓝牙控制台就就可以连接了。连接上以后，输入

```
screen /dev/rfcomm0
```

就将手机的串口数据发送到电脑屏幕上了。
今天是做了第一步，能连上了就好。Ubuntu 8.10下面的自带的蓝牙还有些问题，譬如目前还不能发送文件给我手机，提示出错，很郁闷。接收貌似也有问题。

还找到一个[讲解的很详细的网站](http://epx.com.br/artigos/pys60.php)，上面的信息就是从这里得到的。

有空在看看Sybmain的Open C/C++是用来干嘛的，听说还有点好玩。

好了，洗脚洗脸，休息一下，准备玩GTA4～

PS:晚上又找到了一个网站：

http://www.martin.st/symbian/
