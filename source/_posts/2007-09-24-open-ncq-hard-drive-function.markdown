---
author: hesicong
comments: true
date: 2007-09-24 04:49:20+00:00
layout: post
slug: open-ncq-hard-drive-function
title: 硬盘打开NCQ功能
wordpress_id: 247
categories:
- 硬件
---

五月才说了不要那么太技术，没办法啊，总不可能把技术写成小说吧……那也太强了。

前些日子买了SATA2硬盘，支持NCQ，但当时重装系统的时候没有考虑到打开NCQ技术，所以感觉系统和以前IDE硬盘时候差不了多远。前些天图书馆看报纸的时候终于知道打开NCQ还需要做一些“手脚”，最麻烦的还是需要重装系统，要不然一开启AHCI，就进不了WINDOWS，直接蓝屏。试了一下果然如此。只好关闭AHCI然后回到模拟IDE模式。

网上搜了一段时间，发现很多文章都是说用IBM的一个驱动，然后就可以不用重装搞定。我想没有那么麻烦吧，真是不负有心人啊，找到了ICH9R下面打开NCQ最方便的方法：

* 下载Intel Matrix Storage 安装程序，我下载的英文版iata76_enu.exe
* 然后运行它，加上参数-a -a:

```
iata76_enu -a -a
```

这步只是解压相应文件而已。

* 运行附件注册表
[download id="11"]
* 把 Program FilesIntelIntel Matrix Storage ManagerDriverIaStor.sys 复制到 windowssystem32drivers 目录下，如果有旧版本就覆盖。
* 重启并打开AHCI
* 完成

这样就打开了NCQ。也不知道系统到底快了多少，应该读取小文件的时候才能发挥威力吧。

准备装个Vista和Ubuntu 7.10测试版玩玩。不知道Vista能得多少分~
