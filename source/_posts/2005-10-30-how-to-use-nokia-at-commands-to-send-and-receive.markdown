---
author: hesicong
comments: true
date: 2005-10-30 10:03:26+00:00
layout: post
slug: how-to-use-nokia-at-commands-to-send-and-receive
title: 如何使用发送和接收Nokia AT指令
wordpress_id: 1416
categories:
- 其他
tags:
- at
- nokia
- Phone
---


以前很困惑于Windows自带的超级终端无法发送和接收Nokia手机的AT指令，所以一直以为Nokia手机无法使用AT指令。

今天用Serial Monitor重新监视了一下玩转手机的通讯过程，发现只要设置RTS=ON和DTR=ON就可以发送和接收AT指令了。超级终端连接的时候只把DTR设置为ON，而RTS没有，这就是为何无法连接的原因。

另外当发送和接收FBUS命令时需要把RTS=OFF和DTR=OFF。呵呵，看来串口这个东西还是挺好玩的：)

有空写一个比较完整的好用的串口工具了。
