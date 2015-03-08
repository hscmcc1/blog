---
author: hesicong
comments: true
date: 2009-09-17 16:25:06+00:00
layout: post
slug: vmware-6-5-ubuntu-9-04-vmtools-proper-installation-methods
title: Vmware 6.5 + Ubuntu 9.04 vmtools正确安装方法
wordpress_id: 2014
categories:
- 软件
tags:
- ubuntu
- vmtools
- vmware
---

由于6.5版本的vmware的vmtools的默认安装存在一些问题，貌似是和新的kernel不怎么兼容，所以无法实现屏幕自动缩放、拖拽等功能。在国外网站上搜索了一下，经过实践，发现这样可以解决问题：

``` bash
sudo aptitude update
sudo aptitude install build-essential linux-headers-$(uname -r)
cd /cdrom
cp -a /media/cdrom/VMwareTools* /tmp/
cd /tmp/
tar -vxzf VMwareTools*.gz
cd vmware-tools-distrib/
sudo ./vmware-install.pl
```