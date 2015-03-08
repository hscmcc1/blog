---
author: hesicong
comments: true
date: 2005-10-30 10:01:58+00:00
layout: post
slug: nokia-mobile-phone-data-transfer-on-the-part-of-some-progress
title: 关于Nokia手机数据传输部分的一些进展
wordpress_id: 1412
categories:
- 其他
tags:
- nokia
- Phone
---


由于以前都研究的是Siemens的手机，所以对西门子的手机的数据方式比较熟悉，也做了PDU解码器编码器、OBEX传输协议等等东西。最近狠下心花了1875元买了一个Nokia 6021手机，准备开始探究Nokia手机的一些数据协议。

我找到一个很好的开源项目gnokii，是专门做Nokia手机发送短信等等的一个软件。其文档提供了FBUS、MBUS的数据传输格式，我觉得应该是研究Nokia手机数据传输的宝贵资料了。

第二步就是找IrDA Monitor、Blueteeth Monitor来监视数据通讯过程，不知道现在有没有这些软件，要不然就要安装Linux来研究gnokii进而取得需要的东西了。
