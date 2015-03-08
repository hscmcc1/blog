---
author: hesicong
comments: true
date: 2010-05-04 15:03:49+00:00
layout: post
slug: list-of-common-serial-port-monitoring-software
title: 常用串口监视软件一览
wordpress_id: 2247
categories:
- 其他
tags:
- monitor
- serial port
- tools
- 串口
---

在工控的开发当中常常要应用到串口，应该说串口通信是最基本的通讯工具。不仅在我们编程的时候可以应用一些简单的串口工具进行调试和监控，在做黑箱分析的时候用这些软件能够将串口的通讯协议抓取下来以便进行分析。

以前我做NOKIA的FBUS协议的分析的时候，就是利用HDDSoft的Serial Port Monitor软件进行抓取数据的，用同样的方法在NOKIA PC Suite上也大获成功。最近使用串口监视的例子是应用在HOLIP变频器的通讯协议的模拟上，其基本功能包括初始化、设置速度、得到速度、关闭变频器几个。通过其他的DEMO程序就能获得串口协议，再通过我们软件的模拟就可以用了。对于我的HOLUX GPS而言，前段时间也知道了它的和电脑的通讯协议，这样我就好写一个程序来获取它存储的轨迹信息了。当然最新的百度地图的更新幅度还是挺大的，连我家门口的路口都改了，但坐标的偏移比较大，要是有一个不偏移的地图就好咯。扯远了，下面来介绍一下我用过的一些软件：

**. HDDSoft Serial Port Monitor
**最新的产品在这里可以找到(http://www.hhdsoftware.com/Products/home/serial-monitor.html)
我最早用的是Serial Port Monitor 3.x版本，以前还做过这个软件的破解。该软件功能强大，能够支持到API级的分析。但唯一不好的就是没有破解，比较昂贵。但所谓物有所值嘛。最近发现这个API级的分析确实有用。举个例子来说：

最近HOLIP变频器的问题可以说说。当时有一套DEMO程序，可以很顺利的操作，但是我写的软件就不行。协议是一样的，波特率、停止位、奇偶校验也都一样。发了数据以后，返回的数据没几下就混乱了。然后变频器返回的数据全是乱码。我当时都在怀疑.NET 2.0的串口通讯是不是有问题咯。后来仔细对比DEMO程序和我的程序，发现Handshake参数不对。但是我们的485转接器上就RX和TX两根线，没有其他的了。查看了一下串口类的说明，决定试试流控制，看看是不是有问题。

SerilaPort的类有三个Handshake枚举：

> None 没有用于握手的控件。
> RequestToSend 使用请求发送 (RTS) 硬件流控制。RTS 发出信号，指出数据可用于传输。如果输入缓冲区已满，RTS > 行将被设置为 false。当输入缓冲区中有更多可用空间时，RTS 行将被设置为 true。
> RequestToSendXOnXOff 同时使用请求发送 (RTS) 硬件控制和 XON/XOFF 软件控制。
> XOnXOff 使用 XON/XOFF 软件控制协议。发送 XOFF 控制以停止数据传输。发送 XON > 控制以继续传输。使用这些软件控制，而不是使用请求发送 (RTS) 和清除发送 (CTS) 硬件控制。

最终，只有RequestToSendXOnXOff才能使连接稳定。很是奇怪。最终可能是变频器的流控制没有自适应，导致上位机和下位机之间的通讯发生错位的。

试想，如果没有这个API级的监控，怎么能够想到握手协议的问题呢。

HDDSoft还有USB监视器等等，如果有需要可以研究研究。

**. SUDT SerialTrace**

下载地址：[http://www.sudt.com/cn/index.html](http://www.sudt.com/cn/index.html)

比较好用的一个软件，国产的，属于小巧玲珑型。SUDT还有很多其他串口相关的软件，可以下载来用用。

还有一些其他开发的小的串口调试助手软件，还是有点用处。只是这些小软件的GUI过于花哨了点(比如背景)，建议这些软件作者把软件整的专业点点，呵呵。

另外串口调试还是需要自己的耐心和运气的，特别是协议分析，合理的运用上述软件，事半功倍。
