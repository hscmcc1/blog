---
author: hesicong
comments: true
date: 2005-07-14 16:06:32+00:00
layout: post
slug: net-platform-pda-infrared-connectivity-mobile-communication
title: .net平台下PDA红外连接手机通讯
wordpress_id: 1242
categories:
- 其他
tags:
- .net
- pda
---


今天终于弄明白了PDA红外如何控制手机通讯了。

其实简单，就是PDA的红外模拟的串口和手机通讯。

* 如何取得PDA红外模拟的串口号呢？以我的HP1937为例，我用注册表编辑器浏览[HKEY_LOCAL_MACHINEDriversBuiltInIRDA2410]可以找到Port，我这里是COM3。串口就找到了。
* PDA上的串口通讯。OpenNetCF里面有很强大的类库，其IO.Serial就是一个很好用的串口类，有VB.NET和C#的例程：[HKEY_LOCAL_MACHINEDriversBuiltInIRDA2410]
* 熟悉AT指令，熟悉PDU编码……一切都搞定了…………


有空就开始做一个PDA上发短信的玩意儿，手机按起来太慢了：)
