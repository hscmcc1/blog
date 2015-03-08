---
author: hesicong
comments: true
date: 2009-08-01 04:49:41+00:00
layout: post
slug: raysource-with-netlimiter-thoroughly-clean-up-a-lot-of-upload-bandwidth
title: 用NetLimiter彻底收拾RaySource大量上传带宽占用
wordpress_id: 1899
categories:
- 软件
tags:
- netlimiter
- raysouce
- 带宽
---

最近“早泄”和“漏点”的Windows 7镜像大多数是RayFile网盘承载的，需要下载一个客户端软件——RaySource。作为P2P软件，上传共享是必不可少的，但是RaySource做得太过分了。软件里面虽然有设置下载带宽和上传带宽，但是下载的时候，你永远看到的都是上传0KB/s，你会发现你的ADSL网络变得特别的卡，上传带宽几乎耗尽，那简直是比迅雷性质还恶劣的软件。

我检查了一下RaySource的后台文件Peer.exe，杀了以后上传下载都不行了。RaySource就是一个界面而已，并不做任何的实际上下载动作。

为了能够将带宽限制住，我首先尝试了有名的“P2P终结者”。但最后试用的结果很令人沮丧，依然不能限制上传带宽。

后来在上网搜索了很久，发现了一款[](/images/others/rpc.jpg)![](/images/others/image/thumb/rpc.jpg)

随后会进入NetLimiter的主程序界面。

[](/images/others/netlimiter.jpg)![](/images/others/image/thumb/netlimiter.jpg)

找到peer.exe一行，将Outgoing下面的Limit一栏打上勾，并点击旁边的默认5.00输入实际需要的数值。我这里输入20.0，因为家里面上传带宽最大50KB/s，还要分给其他程序用。

最后如果想全局限制上传带宽，在你的计算机名(这里是Derekhe-pc)一行的Limit里面填入所需要的数值即可。

这样处理以后，整个网络的速度明显变快了。

小知识：

ADSL网络由于下载和上传不对称，会造成上传带宽吃紧，而现在大多数P2P软件都是用TCP/IP协议会上传带宽中有一部分TCP/IP的握手包和校验包，而这些包如果被其他包占用了，会延迟下载中的TCP/IP包的确认，进而影响下载速度。更为严重是如果超过了一定的时间没有得到确认，那么下载得到的TCP/IP包会被丢弃。最终的效果就是，整个网络变得特别卡。所以在ADSL网络中，将上传带宽限制实际上传带宽的80%效果会比较好。

如何测试最大上传带宽呢？方法是找一个很快的服务器，然后上传一个大文件，看看NetLimiter的上传速度即可。
