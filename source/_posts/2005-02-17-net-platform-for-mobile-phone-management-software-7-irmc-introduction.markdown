---
author: hesicong
comments: true
date: 2005-02-17 15:42:31+00:00
layout: post
slug: net-platform-for-mobile-phone-management-software-7-irmc-introduction
title: .net平台手机管理软件开发(7)——IrMC简介
wordpress_id: 1205
categories:
- 其他
tags:
- .net
- irmc
---

(七)  IrMC简介

要实现通讯薄、日历、便签的同步，需要用到IrDA协议里面的IrMC部分。
IrMC全称为Ir Mobile Communications。它定义了利用IR无线传输的设备之间通讯的规则。IrMC协议文档详细叙述了IrMC的方方面面，可以在我的主页(http://dream-world.nease.net)下载到PDF版本。

下面主要讲述我在开发当中所用到的部分以及实现方法。更为详细的资料请参考IrMC协议。
Phonebook

在手机软件桌面端通讯薄的管理是整个软件的必备功能之一，利用其信息可以方便的和Outlook等软件实现同步，实现更强大的功能。

通讯薄的管理分为读取、删除、增添、修改。通过这几个功能的组合可以实现更为强大的同步功能。下面分条概述。
l         读取

n         读取所有的Entry

使用OBEX的GET命令取得telecompb.vcf文件。

得到的的文件是一个vCard文件，里面包含了所有的通讯薄。其格式为vCard格式。关于vCard的简介及编码器解码器见后。下面给出一个范例：
BEGIN:VCARD
VERSION:2.1
X-IRMC-LUID:1017646
X-ESI-CATEGORIES;CHARSET=UTF-8;ENCODING=QUOTED-PRINTABLE:=E5=AE=B6=E4=BA=BA
N:test
ADR:;;Street;city;;610000;country
ORG:company
TEL;HOME:123456
TEL;WORK:123456
TEL;CELL:123456
TEL;FAX:123456
TEL;FAX;HOME:123456
EMAIL;INTERNET:a@a.ao
EMAIL;HOME;INTERNET:b@g
[URL:http](http://blog.csdn.net/hesicong/admin/http)
BDAY:1985-04-23
END:VCARD

也可以通过X-IRMC-LUID所给的LUID号码直接取得相应的vCard。

1.         读取指定LUID的Entry

使用OBEX的GET命令取得telecompbluidxxxx.vcf，其中xxxx代表了LUID号码。

得到的依然是一个vCard文件，只不过只包含特定LUID号码的vCard信息。可以简单的通过Outlook查看vCard所包含的信息。

l         删除

欲删除一个Entry
1.         连接到IrMC_Sync_Service(发送0x80, 0x0, 0x13, 0x10, 0x0, 0x40, 0x6, 0x46, 0x0, 0xC, 0x49, 0x52, 0x4D, 0x43, 0x2D, 0x53, 0x59, 0x4E, 0x43)使手机处于同步状态。
2.         得到ChangeCount
ChangeCount是手机里面储存修改次数的一个数值，主要用于同步。具体相关资料请参考IrMC协议。

3.         构建数据包。
Opcode：PUT
NameHeader：xxxx.vcf(xxxx为欲删除的entry的LUID)
AppParam：0x11，ChangCount字符串形式长度(Integer)，ChangeCount的ANSI形式
4.         使用OBEX的PUT命令，传输一个xxxx.vcf(xxxx指LUID)空文件到telecompbluid覆盖即可。
5.         发送0x81, 0x0, 0x3断开IrMC_Sync_service

l         添加

欲添加一个文件：
1.         连接到IrMC_Sync_Service
2.         得到ChangeCount

3.         构建数据包
Opcode：PUT
NameHeader：.vcf
AppHeader：0x11，ChangeCount字符串长度(Interger)，ChangCount的ANSI形式
Body或者End-of-Body Header：vcf文件内容

4.         OBEX的PUT命令上传到telecompb.vcf文件即可。

注意：文件名取名为“.vcf”。
5.         发送0x81, 0x0, 0x3断开IrMC_Sync_service

l         修改

修改过程与添加过程相似，只是将文件名改为欲修改的vCard的LUID.vcf就行了。

Notes

便签是大多数手机都提供的功能，能够方便的记录简短的信息。在我的M55手机上能够储存150Byte的信息，也就是150个英文或者75个汉字。软件通过管理便签可以与Outlook等软件同步，实现更高级的功能。

管理Notes的方法和Phonebook类似。得到全部Notes的vNote只需要获取telecomnt.vnt即可。删除、添加、修改只需要把telecompbluid改为telecomntluid即可。在此不再赘述。
Calendar

日历功能提供了事件提醒功能，分为重要记事(vEvent)和任务(vTodo)。通过管理日历，同样可以实现和Outlook同步，实现电脑和手机的同步。

管理Calendar的方法和Phonebook类似。得到全部vCalendar只需要获取telecomcal.vcs。删除、添加、修改只需要把telecompbluid改为telecomcalluid即可。在此不再赘述。

但值得注意的是vCalendar的结构
BEGIN:VCALENDAR
VERSION:1.0
BEGIN:VEVENT
….
END:VEVENT
BEGIN:VTODO
…
END:VTODO
END:VCALENDAR

完整的vCalender包含了至少一个vEvent或者一个vTodo，因此在添加、修改vEvent、vTodo时要将其补充为一个完整的vCalender结构，否则服务端会拒绝操作。


遇到的问题：

在实际操作中，遇到问题最多的地方在Phonebook部分。我的手机第一次同步的时候经常出现数据库被锁的情况，用SiMoCo读取也是一样，说明是手机拒绝写操作。这时候关闭手机再重新启动就好了。至于原因，我还没有搞清楚，希望能有高人指点。
