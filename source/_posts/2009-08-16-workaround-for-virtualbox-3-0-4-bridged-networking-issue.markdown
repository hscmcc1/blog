---
author: hesicong
comments: true
date: 2009-08-16 07:59:12+00:00
layout: post
slug: workaround-for-virtualbox-3-0-4-bridged-networking-issue
title: Workaround for VirtualBox 3.0.4 Bridged Networking Issue
wordpress_id: 1915
categories:
- 软件
tags:
- bridged
- ubuntu
- virtualbox
---

I have experienced a bridged networking issue in VirutalBox 3.0.4 with Vista as host and Ubuntu 8.10 as guest.

My networking is installed as following:

A TP-LINK WR340G router configured with DHCP on; Vista Host system linked to this router and can get IP automatically; Ubuntu 8.04 refreshly installed in VirtulBox 3.0.4.

After I installed VirtualBox Addition, guest networking seemed hard to get IP from router. Somestimes it could get a IP. In most time, the networking configuration said that "the link was disconnected".

I searched and someone said that change guest's network card to "Intel PRO/1000 MT Desktop" might solve the problem. I tried it and successed.

If you have this problem, try to change your guest's card type.
