---
author: hesicong
comments: true
date: 2007-07-21 05:26:23+00:00
layout: post
slug: finally-carbide-c-1-2-configured
title: 终于把Carbide.c++ 1.2配置好了
wordpress_id: 224
categories:
- 其他
tags:
- s60
---


S60开发不容易啊，下载了S60 3rd SDK，还有Carbide.c++ 1.2 OEM Edition，弄了一上午才终于能够在BUILD和DEBUG了。Carbide.c++ 1.2整合了OEM, Professional, Developer, Express四个版本，通过licence区分各个版本。OEM版本功能最强大，包括了在线调试的功能。

昨天重装系统之前我安装了1.2版，也能成功调试了，但是重装系统之后，竟然编译都无法通过！难道真正是人品用完了？

重装SDK和Carbide，也不行。后来才想到，是不是选择Carbide的安装版本时选成了Professional(我觉得这个版本已经够我用了)，而下载的破解的licence是OEM的缘故。然后又重装一次，选成了OEM，结果就对了。

呵呵，真是神奇啊~
