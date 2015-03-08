---
author: hesicong
comments: true
date: 2005-11-06 09:58:20+00:00
layout: post
slug: repair-damaged-msdn-browser
title: 修复损坏的MSDN浏览器
wordpress_id: 1410
categories:
- 其他
---


今天弄得我很郁闷，VS2005的MSDN文档无法打开了，现象为点击以后无任何反应。重新安装MSDN未果，使用Repair选项未果。

经过搜索，发现C:Program FilesCommon FilesMicrosoft SharedHelp 8Microsoft Document Explorer 2005目录里面有一个Install.exe，是Microsoft Documents Explorer 2005的安装程序。遂安装看看，使用其Repair选项，几分钟后，MSDN恢复如初！
