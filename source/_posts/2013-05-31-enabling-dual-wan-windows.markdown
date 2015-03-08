---
author: hesicong
comments: true
date: 2013-05-31 04:10:10+00:00
layout: post
slug: enabling-dual-wan-windows
title: Enabling Dual Wan on Windows
wordpress_id: 3263
categories:
- 生活
---

All you gotta do is add some keys into your registry and you'll be set!

Browse into:
HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNetBTParameters

and add these Dword Keys/Values:
RandomAdapter = 1
SingleResponse = 1

Reboot your computer.. and you're set! Plug each modem on your ethernet board and enjoy!
