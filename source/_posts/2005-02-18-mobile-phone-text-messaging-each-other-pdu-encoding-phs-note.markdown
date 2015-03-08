---
author: hesicong
comments: true
date: 2005-02-18 09:00:00+00:00
layout: post
slug: mobile-phone-text-messaging-each-other-pdu-encoding-phs-note
title: 手机——小灵通互发短信PDU编码注意事项
wordpress_id: 1209
categories:
- 其他
tags:
- pdu
---


昨天花了一些时间解决了网友GSM Modem与小灵通发送短信的问题，发现是由于在小灵通号码之前默认加了“+”的缘故。

在 PDU编码中有一个Address Field，其中有一个Address Type段，其值在很多文章里面说固定为0x91。其实这是不对的。按照3GPP 23040-650对于这个字段的说明，0x91是国际通用的，也就是在号码之前加一个“+”号。但对于现在小灵通的 106+区号+号码 这样的格式，将Address Type固定为0x91就变成了 +106+区号+号码 格式，短信中心可能会认为是国际短信，可能发送到其他国家，也有可能发送失败。

所以，对于这种情况，将0x91改成0x81，即可解决。

对于程序的流程，希望能够增加“+”号的判断。如果号码前面有“+”，AddressType值为0x91，否则值为0x81。
