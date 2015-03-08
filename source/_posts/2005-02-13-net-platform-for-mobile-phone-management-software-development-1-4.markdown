---
author: hesicong
comments: true
date: 2005-02-13 15:27:43+00:00
layout: post
slug: net-platform-for-mobile-phone-management-software-development-1-4
title: .net平台手机管理软件开发(1-4)
wordpress_id: 1190
categories:
- 其他
tags:
- .net
- Phone
---

.net平台手机管理软件开发
(一) 简介

通过几个月零零碎碎地学习各方面知识之后在这个大二的寒假笔者终于用VB.Net写出了西门子手机的辅助软件——Siemens Support Tool。虽然我没有最终的完成这个软件的所有设计，但核心的功能已经开发完成，界面也基本到位，我的学习的目的也就达到了。在2月12日正式停止开发的以后，我想到把寒假20几天的辛苦历程作一个总结，为以后重温这部分知识起到一定的作用，也为广大编程爱好者提供一些帮助和启发。笔者才疏学浅，编程也是零碎时间自学过来的，所以有缺陷和谬误请大家斧正。

(二) 为何要设计？设计目的？

用过西门子手机的都知道西门子手机在人性化设计方面做得比较好，但是让笔者失望的是电脑端上使用的官方软件SDS，其缺点是操作不方便，速度比较慢，感觉人性化设计不到位。后来官方推出了用于65系列的Mobile Phone Manager，界面很好看，但安装后大于120MB的容量及其较慢的速度又让人大跌眼镜。
GhostMobile(简称GM)是我用过的国产非官方软件里面比较好的一款，但经常出现传输中一直等待的情况。估计作者并没有超时设计。另外GM文件传输速度很慢，短信管理不方便，后来也由于作者使用了新的手机，也就放弃了GM。后来又找到一款Siemens Mobile Control，简称SiMoCo，是国外的非官方软件。800多kb的身躯及其速度、功能方面都超过了官方及GM，令人刮目相看，一度成为我最喜欢的软件。但用后发现一些问题，软件过于专业，选项太多，对中文的支持不好。

所以最终的目的就是做一款能够实现文件传输、短信、便签、任务、重要记事管理的软件。

(三) 准备工作
2004的暑假我已经做了一部分，实现了基本的文件传输和短信功能，当时取名叫作M55 File Transfer Tool。后来在东北手机网上公开了，有一些GM无法连接的手机我的软件都可以连接，所以到现在为止有些网友还在使用我的这个软件。但由于知识不够，做得不是很理想，经常出现问题。

短信部分是官方网站下载的ATC_Command_Set_For_L55_Platform，详细地讲述了55平台上的AT指令集。其实SMS部分的AT指令各大手机厂商都是通用的，已基本上属于同一的指令集了。但是发现文件传输是OBEX却不是那么简单。

官方并没有任何开发文档说明数据传输使用的是什么协议，我用Serial Monitor监视到了数据传输的过程，全是HEX代码，不知道是什么意思。那段时间一度陷入迷茫状态，不知道如何下手。后来在google搜索，又在CSDN里发了一些帖子求救，但却没有一个能够明确说明的。有一个网友的留言给了我一些线索，他说可能是蓝牙协议里面的部分。

这条线索给我了极大的鼓舞，因为后来，顺藤摸瓜找到了红外线传输协议，意外地发现了IrOBEX的描述协议竟然和监视到的HEX代码的结构一样。随后经过仔细的研究发现就是OBEX协议，此协议可以作为上层协议用在红外线协议、蓝牙协议等。此过程大约经历了2个多月。其实现在看来这个问题简单了，手机的工厂模式的串口监视里面就会显示当前使用的协议。当数据传输开始时，会自动从GIPSY变为OBEX。但那个时候哪知道呢？

跨越了OBEX协议的障碍以后我写了一个OBEX-Multithread类，写得很垃圾，把十六进制转换成字符串，然后再转回来在发送。中间使用了string作字符串操作，速度很慢，测试以后只能勉强超过GM的传输速度。

后来借着Serial Monitor监视GM读取手机通讯薄的原理，发现通讯薄是在telecompb目录里面，但是这个目录在手机里面是隐藏的，无法直接访问。由于原来写得OBEX库很糟糕，只能对应文件传输，对于这个特殊文件夹里面的文件都无法操作。修改了之后效果不好，遂放弃了OBEX-Multithread。

由于学习的原因，中途也只得停下来准备期末考试和六级。中途无聊的时候研究IrMC里面的vCard、vNote、vCalc格式，基本弄懂了如何同步通讯薄、便签、日历。2005年1月14日，放假回家了就正式开始动工，把所有的东西都重新写，对我来说，这是一个巨大的挑战。

(四) AT指令简介
AT指令在当代手机通讯中起着重要的作用，能够通过AT指令控制手机的许多行为，包括拨叫号码、按键控制、传真、GPRS等。西门子M55手机为我提供了很多的AT指令，网络上关于AT指令的资料也很多，我这里提取一些比较重要的做个简单解释。其他的手机也基本上通用，更详细的资料请查阅手机生产商的资料。

欲使用AT命令，可以安装微软的超级终端程序，选择好端口连接速度以后就可以正常使用了。

AT指令用法

.  测试命令(Test Command)

在AT指令后面加上“=?”即构成测试命令。

例如“AT+CSCS=?”会列举出所有支持的字符集。

.  读取命令(Read Command)

在AT指令后面加上“?”即构成读取命令。

例如“AT+CSCS?”会列举出当前设置。

.  执行命令(Execute Command)

一般而言在AT指令后加上“=”及命令参数即可。有些命令例如AT+CMGR命令没有参数，直接就可以执行。

注：并不是所有的AT指令都支持1和2。


常用基本AT指令

<table style="border: medium none; width: 100%; border-collapse: collapse;" border="1" width="100%" cellpadding="0" cellspacing="0" class="MsoTableGrid" >
<tbody >
<tr >

<td width="18%" style="border: 1pt solid windowtext; padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >

命令

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

作用

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

测试连接是否正确

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
ATE0

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

关闭回显。程序初始化AT部分首先关闭回显。

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
ATE1

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

打开回显。使用超级终端测试命令时打开。

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CGMI

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

得到厂商信息

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CGMR

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

得到手机版本号

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CGSN

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

得到手机序列号(IMEI)

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CIMI

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

得到手机IMSI号码

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CSCS

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

获取、设置手机当前字符集。可设置为GSM或UCS2

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CBC

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

获取手机电量

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CCLK

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

获取设置手机时钟

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CNUM

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

机身号码。分为线路一和线路二

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CSQ

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

当前信号

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+COPS

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

网络营运商

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CSCA

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

短信中心号码

</td>
</tr>
</tbody></table>

以上这些指令都用于与手机连接的时候初始化用。取得手机IMEI及IMSI可以给使程序支持更多的手机连接并且保持数据独立。

短信部分

<table style="border: medium none; width: 100%; border-collapse: collapse;" border="1" width="100%" cellpadding="0" cellspacing="0" class="MsoTableGrid" >
<tbody >
<tr >

<td width="18%" style="border: 1pt solid windowtext; padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >

命令

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

作用

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CPMS

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

选择短信储存地点。可选择ME(SIM卡)和MT(机身)

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CMGL

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

列出指定状态的短信息的PDU代码

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CMGR

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

列出指定序号的短信息PDU代码

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CMGS

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

发送短信

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CMGD

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

删除指定的短信

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CMGF

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

短信格式。分为Text模式和PDU模式

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CNMI

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

设置新短消息通知电脑端

</td>
</tr>
<tr >

<td width="18%" style="padding: 0cm 5.4pt; width: 18.16%; background-color: transparent;" valign="top" >
AT+CSCA

</td>

<td width="81%" style="padding: 0cm 5.4pt; width: 81.84%; background-color: transparent;" valign="top" >

短信中心

</td>
</tr>
</tbody></table>

以上命令是短消息部分最经常使用的命令。具体条目及使用方法会在后面重点讲解。
