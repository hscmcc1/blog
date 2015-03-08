---
author: hesicong
comments: true
date: 2010-02-02 14:08:54+00:00
layout: post
slug: gdebugger-universal-crack-method
title: gDebugger通用破解方法
wordpress_id: 2143
categories:
- 其他
tags:
- crack
- gDebugger
- opengl
---

下载0day的5.2.1的破解(地址网上找哈)，得到lic_gen.exe用于生成文件。

下载最新的5.4.0，安装，将gDEBuggerAppCode.dll中的RSA字符串(字串)：

C4DD47CE9C8B14AD4993751B8E92598765868B23653DF74B06EF9131EFAEE53013760709FF932832C244AED24E1371673BA091BF5D04CE1A6ADA20378F81CDC912200FD6ADB517BA4CD32164D6D905E9B713406A1416295D4DEB8BFE769A3E46A2A20F1B36BBA06D17D3C2118A25A134EBC7B5A0412308422724BBE81635F9BB

替换成

CB64B8141DA2441B39902D0C890BE6C6C668B7D0C69275EC4DC55BC0583BF89FCB1B978F5F11558A0FA0910D19EF8ACE6E074CAF31DE7DADF768878CE2EF456FF9A1967F7F594E6D0B6AEF92CB73B7534ADE8383BD537698419E9F1CFC21D873FB1F6D5038603D1E68A8ADF3CB9511475D865454421FAEFB723A9C6BDA1DF0A9

并保存。这是代替key_replacer的作用(这个程序只针对5.2.1)

覆盖原文件后，按照5.2.1破解操作进行。用lic_gen生成Licence。注意生成Licence的时候选择第一个。
进入后不会提示需要注册的窗口，看似完全破解了。按“播放键”进行调试会没有反应，不清楚为啥。
退出程序，打开OllyDbg，选择File->Open，选择gDebugger.exe，然后按F9，就可以用了。注意将OllyDbg的Debugging Options中的Exceptions的Ignore (pass to program) following exceptions所有选项勾上。防止在pause程序的时候被OllyDbg中断。

这时就是真正的完全破解版。目前5.2.1和5.4.0均可以使用此方法破解。

另外，如果达人有能力会破解的帮忙看看为什么用OllyDbg载入后他的限制就消失了，应该会有一个检测调试器的函数怎么的，把这个函数破解了就好办了。

如需要原版和破解的压缩文件，请留下你的联系方式。

Enjoy!
