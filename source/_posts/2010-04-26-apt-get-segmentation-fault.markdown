---
author: hesicong
comments: true
date: 2010-04-26 10:27:31+00:00
layout: post
slug: apt-get-segmentation-fault
title: Apt-get segmentation fault
wordpress_id: 2226
categories:
- 其他
tags:
- apt-get
- debian
---

今天升级Debian 5的apt-get，结果升级后apt-get一用就segmentation fault，郁闷死。看来是啥子指针出问题了。

[这篇文章](http://www.linuxforums.org/forum/debian-linux-help/80216-apt-get-segmentation-fault.html)中提到了一个方法解决：

```
rm -rf  /var/cache/apt/*.bin
```

试了一下，果然有用~
