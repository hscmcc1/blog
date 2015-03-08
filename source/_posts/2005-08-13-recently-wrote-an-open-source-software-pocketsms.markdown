---
author: hesicong
comments: true
date: 2005-08-13 09:41:04+00:00
layout: post
slug: recently-wrote-an-open-source-software-pocketsms
title: 最近写的一个开源软件——PocketSMS
wordpress_id: 1372
categories:
- 其他
tags:
- .net
- pda
---


这是我自己写的一个短信发送接收软件，主要是为了解决自己的HP1937和SIEMENS CXV65连接发送短信的问题。之前，我试用过Mphone等软件，都不能很好的支持这个手机。后来就自己写了一个。

这是我为我自己的需要写的一个精简版。原本打算做一个更强的版本的，但现在看来时间不足，精力也不足，也只好暂时放弃了。

现在此软件能够获取SIM卡上的通讯薄和PocketOutlook上的通讯薄，能够群发短信(可能有BUG)，自动接收状态报告和新来短信，自动存储短信历史到文本文件，方便管理。

程 序使用VB.Net书写，由于用到了一个非托管的PocketOutlook.dll，而这个dll是为ARM机型写的，所以暂时只支持ARM。其他手机 我没有测试过，我看了一下AT指令，理论上SIEMENS和NOKIA的手机都能使用。只需要复制压缩包内文件到任意目录即可使用。另外建议安装.net compact framework sp3，修正了很多问题并能够明显的加快.net cf应用程序速度。

第一次运行请进入设置，然后选择端口号。一般红外连接选COM3，蓝牙可能需要配对后使用COM6(我无设备测试)，连接速度115200即可。

软件为绿色，如果出现问题，删除data目录即可以重新开始。


此软件为我自己而写，如果有需要请自行修改源代码，呵呵；)


下载：
[[download id="27"]
](http://files.cnblogs.com/hesicong/PocketSMS.rar)
