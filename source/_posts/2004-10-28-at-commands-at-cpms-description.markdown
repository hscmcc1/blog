---
author: hesicong
comments: true
date: 2004-10-28 15:19:05+00:00
layout: post
slug: at-commands-at-cpms-description
title: AT指令：AT+CPMS介绍
wordpress_id: 1178
categories:
- 其他
tags:
- at
---

这是我在SIEMENS AT COMMAND SET里面摘录的一段：

AT+CPMS Preferred SMS message storage

Revision according to GSM 07.05 Version 4.7.0

Test command

AT+CPMS=?

Response

+CPMS: (list of supported <mem1>s),( list of supported <mem2>s) ,(list
+of

supported <mem3>s)

Parameter

<mem1>

Memory from which messages are read and deleted

SM SIM message storage

ME Mobile Equipment message storage

MT combination of "ME" and "SM" storages

<mem2> Messages will be written and sent to this memory storage:

SM SIM message storage

ME Mobile Equipment message storage

MT combination of "ME" and "SM" storages

<mem3> Memory in which received messages are preferred to be stored, if

routing to TE is not set (see AT+CNMI command with parameter

<mt>=2)

SM SIM message storage

ME Mobile Equipment message storage

MT combination of "ME" and "SM" storages

Read command

AT+CPMS?

Response

+CPMS:

<mem1>,<used1>,<total1>,<mem2>,<used2>,<total2>,<mem3>,<used3>,<total3

>

Parameter

<memx>

Memory from which messages are read and deleted

<usedx> Number of messages currently in <memx>

<totalx> Total number of messages that can be stored in <memx>

Write command

AT+CPMS= <mem1>[,<mem2>[,<mem3>]]

Parameter

<mem1>

See Test command

<mem2> See Test command

<mem3> See Test command

+CPMS: <used1>,<total1>,<used2>,<total2>,<used3>,<total3>

OK/ERROR/+CMS ERROR

Notes 1) The Mobil Equipment storage "ME" has capacity for 25 short messages

2) The storage "MT" is a combination of "ME" and "SM" storage. If "MT" is

chosen indices < 26 refer to ME" storage while indices 26 or higher are

associated with the "SM" storage

3) Incoming short messages with message class 2 (see GSM 03.38) will be

stored in the "SM" storage only. Therefore, the AT^SMGO:2 indication (see

AT^SMGO command) can occur without a preceding AT^SMGO:1 indication.

一些概念：

MEM1：读取和删除短信所在的内存空间。
MEM2：写入短信和发送短信所在的内存空间。
MEM3：接收到的短信的储存位置。

具体用法：

. 语句：
AT+CPMS=?
作用：

测试命令。用于得到手机所支持的储存位置的列表。在我的SIEMENS M55手机上返回：
AT+CPMS=?
+CPMS: ("MT","SM","ME"),("MT","SM","ME"),("MT","SM","ME")

表示手机支持MT(手机终端),SM(SIM卡),ME(手机设备)

. 语句：
AT+CPMS=”MT”,”ME”,”SIM”
设置MEM1为MT，MEM2为ME，MEM3为SIM

. 语句：
AT+CPMS?

作用：
得到当前的设置。
例如通过第二条语句的设置以后返回：

AT+CPMS?
+CPMS: "MT",7,150,"ME",7,100,"SM",0,50

表示MT里面有7条短信，总共可以储存150条。ME里面有7条，共可以储存150条。SIM卡上可以储存50条。
