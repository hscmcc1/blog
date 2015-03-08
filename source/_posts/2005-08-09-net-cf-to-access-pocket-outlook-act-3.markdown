---
author: hesicong
comments: true
date: 2005-08-09 09:39:38+00:00
layout: post
slug: net-cf-to-access-pocket-outlook-act-3
title: .Net CF访问Pocket Outlook法三则
wordpress_id: 1370
categories:
- 其他
tags:
- .net
- pda
---


近日做的软件需要与Outlook进行交互，很不幸的是.Net CF没有提供直接的方法。上网Google了一下，总结出两个方法：
*  最简单的方式——使用商用InTheHand Outlook组件，代价是很明显的……$49。微软的MSDN里面提供的也是适用InTheHand的东西，显然对于我们来说此法作废。……
*  最复杂的方法——自己开发.Net下POOM(Pocket Outlook Object Model)。后来想想这需要涉及到太多的东西，不划算。
*  再找找，绝对有免费的——功夫不负有心人——外国朋友EGILH的Blog上贴出来一篇文章——[Using the POOM from .NET CF 1.0](http://www.egilh.com/blog/archive/2004/09/27/210.aspx)。

文章中写道了用POOM C++ warapper来访问的方法。他提到了[.NET Compact Framework Sample: POOM wrapper](http://www.microsoft.com/downloads/details.aspx?FamilyId=80D3D611-CC81-4190-AAB4-B1EA57637BAC&displaylang=en)，是封装了的POOM代码，其中DEMO代码提供了一个.Net CF client，提供了一些基本的功能。正好，我需要的读取联系薄的功能就在里面：)

第一次运行代码之前记得要找到sample的安装目录并把里面的Cab包安装到PPC上，内包含了POOM C++ wrapper的dll，要不然.Net CF Client运行时会出现MissingMethod错误。
……赫赫，终于找到免费的了，用起来还不错……~

关于如何扩展.Net CF client，EGILH的文章也说得很清楚了，大家自己钻研了：)
