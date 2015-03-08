---
author: hesicong
comments: true
date: 2012-11-10 03:05:06+00:00
layout: post
slug: hp1018_ubuntu_10_04_an_zhuang_zhi_dao
title: HP1018 Ubuntu 10.04 安装指导
wordpress_id: 3176
categories:
- 软件
---

惠普打印机在Ubuntu下 需要安装opensource的驱动，郁闷的是hplip.sf.net在伟大的CN是访问不了的，所以驱动下载fireware的时候就死掉了。所以只好翻墙过去看看怎么回事。

通过hp-plugin -g可以发现驱动是到hplip.sf.net/plugin.conf里面取东东，那么翻墙过去看看这里有什么：

```

 [3.12.10]
 url = http://www.openprinting.org/download/printdriver/auxfiles/HP/plugins/hplip-3.12.10-plugin.run
 size = 1808025
 timestamp = 1349329318.15
 datetime = Thu, 04 Oct 2012 05:41:58 +0000
 checksum = ecac4ce4613db41a995df81b2efcbd3e502433ae
 num_files = 52
 revision = 17299

 [3.12.11]
 url = http://www.openprinting.org/download/printdriver/auxfiles/HP/plugins/hplip-3.12.11-plugin.run
 size = 1808048
 timestamp = 1352274536.66
 datetime = Wed, 07 Nov 2012 07:48:56 +0000
 checksum = 3376764cf781792662200aa61feaae1cc44f30b1
 num_files = 52
 revision = 17490
```

看吧，那么多地址。

那么，我们自己去访问看看：

http://www.openprinting.org/download/printdriver/auxfiles/HP/plugins/

里面有很多驱动，找最新版本的plugin就可以了
