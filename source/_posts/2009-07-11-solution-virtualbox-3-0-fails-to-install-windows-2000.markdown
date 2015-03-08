---
author: hesicong
comments: true
date: 2009-07-11 16:54:46+00:00
layout: post
slug: solution-virtualbox-3-0-fails-to-install-windows-2000
title: 解决VirtualBox 3.0安装Windows 2000失败
wordpress_id: 1870
categories:
- 软件
tags:
- vbox
- virtualbox
- Win2k
- Windows 2000
- 安装失败
---

最近在试用VirtualBox 3.0遇到了极大的问题，Windows 2000没法顺利安装，表现为在安装的最后阶段出现虚拟机自动重启的现象。我原来以为是IOAPIC等配置的原因，结果一一试过后都不能解决问题。最后只有在Bug系统上报告这个问题了(http://www.virtualbox.org/ticket/4474)

今天一回来就得到了回复，很快，就一句话：

This is documented in the manual. There's a workaround mentioned in chapter 11 iirc.

果然，我找到了原文如下：http://www.virtualbox.org/manual/UserManual.html#id2531590

>
> ### Windows 2000 installation failures
>
> When installing Windows 2000 guests, you might run into one of the       following issues:
>   * Installation reboots, usually during component           registration.
>   * Installation fills the whole hard disk with empty log           files.
>   * Installation complains about a failure installing           `msgina.dll`.
> These problems are all caused by a bug in the hard disk driver of       Windows 2000. After issuing a hard disk request, there is a race       condition in the Windows driver code which leads to corruption if the       operation completes too fast, i.e. the hardware interrupt from the IDE       controller arrives too soon. With physical hardware, there is a       guaranteed delay in most systems so the problem is usually hidden there       (however it should be possible to reproduce it on physical hardware as       well). In a virtual environment, it is possible for the operation to be       done immediately (especially on very fast systems with multiple CPUs)       and the interrupt is signaled sooner than on a physical system. The       solution is to introduce an artificial delay before delivering such       interrupts. This delay can be configured for a VM using the following       command:
>
>
>
> VBoxManage setextradata VMNAME "VBoxInternal/Devices/piix3ide/0/Config/IRQDelay" 1
>
>
>
> This sets the delay to one millisecond. In case this doesn't help,       increase it to a value between 1 and 5 milliseconds. Please note that       this slows down disk performance. After installation, you should be able       to remove the key (or set it to 0).
>
>
Windows 2000 安装失败

当作为客户端安装Windows 2000时，你可能回遇到如下的问题：

* 安装重启，通常发生在组件注册这个环节
* 安装将整个磁盘用空的日志文件塞满
* 安装时报告安装msgina.dll失败

这些问题都是由Windows 2000的硬盘驱动的一个bug造成。在一个磁盘请求以后，在Windows的驱动中会出现一种竞争状态， 这种状态将会导致崩溃如果操作完成得过于迅速，例如来自于IDE控制器的硬件终端来得太快。在物理硬件上，在大部分系统上都有有一个可以得到保证的延迟， 所以这个问题通常隐匿了起来(但是在物理硬件上也可能可以重现)。在虚拟环境中，操作系统可能会立刻完成这个请求(特别是在非常快的多CPU环境中)并且 中断信号会比物理系统上来的早。解决方案是在这种中断前加人为的延迟。这种延迟可以使用下面的命令进行配置：

>
> VBoxManage setextradata VMNAME "VBoxInternal/Devices/piix3ide/0/Config/IRQDelay" 1
>
>

这将设置1毫秒的延迟。如果这都还不能解决问题，增加这个值于1到5毫秒之间。注意这会降低磁盘性能。在安装完以后，你可以将这个键值去掉(或者设置为0)
补充：VBoxManage命令在你的VirtualBox目录下面，在运行中输入cmd然后切换到VirtualBox目录就可以操作了。
