---
author: hesicong
comments: true
date: 2008-12-26 15:18:43+00:00
layout: post
slug: easy-streaming-media-server-built-linux
title: 搭建Linux简易流媒体服务器
wordpress_id: 1496
categories:
- 软件
tags:
- Linux
- vlc
---

在教研室一直都用笔记VNC远程连接到寝室的台式机，图像倒是没有问题了，可郁闷的是声音没有。倒是突然觉得windows的远程还好用了些，包括能够把本地磁盘带到远程的功能都是挺好用的。唯一不满意的就是Windows的稳定性，万一崩溃了就麻烦了，而在开发中遇到这些问题也是相当头痛的。扯远了——言归正传，回到我们的流媒体服务器上来。

为什么我要弄一个流媒体服务器呢，关键是我想学学英语，在教研室开发的时候听听英语也是好的。但是直接听下载的英语，一来麻烦，而来下载了一怕啦，实际听得时间并不多。还是喜欢广播，一直有新鲜的东西能够听到，量大，而且随时可以收听，不用担心找不到资源。http://english.cri.cn/就提供了很多有用的资源，通过mms流媒体服务播出。那么，如何将这个音频带到远程的电脑上呢？

在Linux下面让远程能够听到声音，我查到可以有两种方法：

1. 将本地声音转到eSound声卡驱动，然后远程链接播放。用了一下感觉比较麻烦，没搞懂怎么弄得。
2. 如果你只是播放一些音乐的话，用vlc media player就可以搞定。

首先，将解码器等安装全：

``` bash
sudo apt-get install ubuntu-restricted-extras
```

然后安装vlc

``` bash
sudo apt-get install vlc
```
就可以了。在gnome菜单上能够找到vlc media player。另外请注意，我台式机用的是ubuntu 8.10，所以vlc的版本可能和你所用的版本有所不同，我笔记本的是ubuntu 8.04，两者界面不同。

[](/images/others/screenshot-wwwcrienglishcom-easy-fm-vlc-media-player.png)![](/images/others/image/thumb/screenshot-wwwcrienglishcom-easy-fm-vlc-media-player.png)

选择，媒体——》串流——》输入你的mms地址，然后点击串流。当然，如果你想将本地的音乐转到外面播放，也可以选择你的本地文件。

[](/images/others/screenshot-vlc-open.png)![](/images/others/image/thumb/screenshot-vlc-open.png)

选择你的IP地址和端口，然后就可以播放了。

[](/images/others/screenshot-vlc-screamout.png)![](/images/others/image/thumb/screenshot-vlc-screamout.png)

在远程计算机上打开vlc或者其他支持流媒体的播放器，然后输入你配置的地址就可以了。这里在远程输入http://218.194.34.231:1234就可以了。呵呵。

另，在远程的控制台下想调节音量，输入alsamixer就可以调节了～：)
