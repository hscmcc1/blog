---
author: hesicong
comments: true
date: 2005-02-18 15:47:13+00:00
layout: post
slug: net-platform-for-mobile-phone-management-software-9-introduction-and-sms-pdu-format-parts
title: .net平台手机管理软件开发(9)—— 短信部分之PDU简介及其格式
wordpress_id: 1212
categories:
- 其他
tags:
- .net
- pdu
---

**(九) ****短信部分——PDU****简介及其格式**
PDU是大多数手机短信通讯的核心，仅有少数手机只支持Text模式(例如笔者的MOTO C330)。PDU模式比起Text模式可以提供能为强大的功能，但其编码较Text模式困难。无论哪种模式，我们都可以通过AT指令控制终端实现短信的发送、接收、删除等管理。下面主要介绍PDU的构成及编码解码。
**PDU****的构成**
PDU是由一串由“0-9”及“A-F”组成。表面上看起来就是一组16进制的数所组成的。

下面举一个发送和接收的例子。
.  手机发送的一个PDU串：
0891683108200805F011190D91683188902848F40008FF108FD9662F4E0067616D4B8BD577ED4FE

对比3GPP协议得到：(二进制代码从左到右依次为高位->低位)
**短信中心地址字段**
**08** 地址长度：8个字节，包括其后的91

**91** 地址类型：10010001
Bit7：1。始终为1
Bits 6,5,4：Type-of-Number(号码类型)：001，代表Internation Number。也即是号码前加“+”。**注意**：对某些比较特殊的号码，例如手机与小灵通的互通时，这里不能设置为001，而要设置成000，代表号码前没有“+”，否则无法接收。
Bits 3,2,1：Numbering-plan-identification：一般默认为0001，表示电话号码类型的。
**683108200805F****0** 短信中心号码：一个字节内反转，8613800280500，如果长度为奇数则需要加“F”补齐
** FirstOctet****字段**
**11 **包含TP-MTI(2bit)，TP-RD(1bit)，TP-VPF(2bit)，TP-RP(1bit)，TP-UDHI(1bit)，TP-SRR(1bit)

二进制表示形式：0 0 0 10 0 01
**TP-MTI****：01**
TP-Message-Type-Indicator(消息类型指示符)

Bit1,0：01 指示为SMS-SUBMIT类型
**TP-RD****：0**
TP-Reject-Duplicates(是否拒绝相同重复消息)
Bit2：0 指示短消息中心接受未转发的具有相同TP-MR的消息。
**TP-VPF****：10**
TP-Validity-Period-Format(有效期格式)
Bit4,3：10 指示使用相对格式。
**TP-SRR****：0**
** **TP-Status-Report-Request
Bit5：0 指示不使用状态报告。

**TP-UDHI****：0
** TP-User-Data-Header-Indicator(用户数据头标示)
Bit6：0 指示这是一个SMS消息，没有用户数据头。EMS消息需要设置。

**TP-RP****：0
** TP-Reply-Path(回复路径)
Bit7：0 指示没有设置回复路径。
**消息参考值TP-MR**
19 TP-Message-Reference
** ****对方号码字段**
** **0D91683188902848F4

其结构与短信中心号码字段部分类似，不再赘述。
**协议****标识****TP-PID**

** **00 TP-Protocol-Identifier(上层协议指示)，一般设置为00，表示普通GSM，点对点
** ****编码方法TP-DCS**
08 TP-Data-Coding-Scheme(数据编码设置)，指示TP-UD的编码方式。08代表Unicode方式。00为7Bit编码
** ****有效期TP-VP**
FF TP-Validity-Period(有效期)。FF表示最大。
**用户数据长度TP-UDL**
10 TP-User-Data-Length(用户数据长度)****
0x10长度。注意不同编码下用户长度定义不同。

用户数据TP-UD****

8FD9662F4E0067616D4B8BD577ED4FE TP-User-Data

中文“这是一条测试短信”的Unicode编码

.  手机接收的PDU串
0891683108200805F0040D91683188902848F4000850208151754500108FD9662F4E0067616D4B8BD577ED4FE1
**短信中心地址字段**
** **0891683108200805F0：+861380280500
**FirstOctet**
**** 04

其二进制代码：00000100
TP-MTI：00
TP-MMS(TP-More-Message-to-Send)：1 短信中心没有更多的消息发送
TP-SRI：0
TP-UDHI：0
TP-RP：0
**发送方号码**
0D91683188902848F4：+8613880982844

**协议标识
** 00 TP-DCS 点对点
**编码方式**
08 TP-DCS Unicode编码


短信中心时间戳
50208151754500 TP-SCTS 字节反转05/02/18 15：57：45 最后的00代表时区，这里为0

用户数据长度
10 TP-DHL

用户数据
8FD9662F4E0067616D4B8BD577ED4FE1 TP-UD

中文“这是一条测试短信”的Unicode编码
