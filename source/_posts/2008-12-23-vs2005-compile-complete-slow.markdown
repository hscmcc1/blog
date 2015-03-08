---
author: hesicong
comments: true
date: 2008-12-23 14:07:18+00:00
layout: post
slug: vs2005-compile-complete-slow
title: VS2005编译完成缓慢
wordpress_id: 1487
categories:
- 其他
---

今天把虚拟机的系统换成了WinXP，Win2000系统确实有些问题，不得以而为之。但很令人费解的是项目编译的时候总是很慢，每次都是在编译完成的最后一刹那停住了，然后要等几秒钟才能缓过气来。以前编译控制台项目都没有遇到过这样的问题。逐步排除后才发现，原来只要打开过Windows Forms编辑器，就会造成编译缓慢。

回想以前用C++/CLI做东西的时候，就是感觉IDE特别的缓慢，竟然这下弄C#工程也遇到了这个问题。网上搜索一番没有发现解决方法。后来经过试验，只要把解决方案关掉，在打开，就OK了。如果再次打开了Windows Forms编辑器，又会变慢，真是郁闷。
