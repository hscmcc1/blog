---
author: hesicong
comments: true
date: 2005-02-17 15:44:05+00:00
layout: post
slug: net-platform-for-mobile-phone-management-software-8-vcard-vnote-vcalender-format-description
title: .net平台手机管理软件开发(8)—— vCard、vNote、vCalender格式简介
wordpress_id: 1207
categories:
- 其他
tags:
- .net
- vcal
- vcard
- vnote
---

(八)  vCard、vNote、vCalender格式简介
vCard称为电子商务卡片，主要用于记录通讯薄的联系人信息等，方面不同设备之间的数据交换。自笔者的M55手机中，可以发送一条短信到对方，其中包含了vCard格式的联系人信息，西门子其他型号的手机可以接收解码存储。另外通过手机红外线传输到电脑上的联系人也是用的vCard格式。如果安装了Outlook，则可以直接打开vCard并看到其包含的信息。下面主要简要介绍一下vCard格式，其他vNote、vCalender格式和vCard相近，就不再赘述。更详细的资料请参考vCard Specification，在笔者主页有相关下载。

关于vCard、vNote、vCalender的.Net简单编码解码器请参阅SIEMENS SUPPORT TOOL源代码中的IrMC部分。
vCard Object(vCard对象)

一个vCard数据流可以包含一个或者多个vCard Object。在数据流中一个vCard Object定义为以“BEGIN:VCARD”开始并以“END:VCARD”结束的数据。如果只有到达了数据流尾都没有出现“END:VCARD”，则整个vCard Object包含从“BEGIN:VCARD”开始到数据流结束的地方。
vCard Property(vCard属性)
vCard是一个或多个Property的集合。一个Property是唯一命名的值。一系列的Property可以在vCard中成为一组。
vCard Property的格式如下：
PropertyName[‘;’ PropertyParameters]’:’PropertyValue

注：
.   PropertyName及PropertyParameters不区分大小写。
.   PropertyParameters可选，可以为零个或多个，与ProperyName以分号相隔，与PropertyValue以冒号相隔。
.   vCard可以分多行呈现。由于在这个软件里面应用得不多，所以笔者也没有钻研具体实现方法。可以参考vCard Specification。

例如TEL;HOME;+86111222333其PropertyName为TEL，PropertyParameters为HOME，PropertyValue为+86111222333。
Encoding(编码)
vCard默认的编码方式是7-Bit。默认编码方式可以使用ENCODING属性参数(Property parameter)改变。其值为可以为BASE64；QUOTED-PRINTABLE；8BIT。这个参数可以用在任何的Property里。

例如：
X-ESI-CATEGORIES;CHARSET=UTF-8;ENCODING=QUOTED-PRINTABLE:=E5=AE=B6=E4=BA=BA

下面简要说明QUOTED-PRINTABLE编码方式，更为详细的资料请参考相关文档：
ASCII可显示字符基本保持不变。Unicode字符或者UTF8编码字符使用等号加其对应16进制代码表示。例如上述CHARSET为UTF8的字符=E5=AE=B6=E4=BA=BA对应的UTF8编码0xE5，0xAE，0xB6代表中文“家”，其他的代表“人”。另外如果其中有可显示ASCII码，保持原样输出。

例如ENCODING=QUOTED-PRINTABLE:Home=E5=AE=B6People=E4=BA=BA

解码后为“Home家People人”。
Character Set(字符集)

默认的字符集是ASCII。可以通过CHARSET参数改变默认的字符集。其参数可取的值为所有IANA(Internet Assigned Numbers Authority)注册的字符集。这个参数可以用于任何Property，但某些Property并不起作用。

例如：
X-ESI-CATEGORIES;CHARSET=UTF-8;ENCODING=QUOTED-PRINTABLE:=E5=AE=B6=E4=BA=BA
vCard例子：
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
URL:http
BDAY:1985-04-23
END:VCARD
