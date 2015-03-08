---
author: hesicong
comments: true
date: 2006-11-18 03:25:44+00:00
layout: post
slug: ubuntu-6-10-sources
title: Ubuntu 6.10 Sources
wordpress_id: 88
categories:
- 软件
tags:
- ubuntu
---

To add these sources, edit /etc/apt/sources.list

* Archive.ubuntu.com 更新服务器(欧洲)：

deb http://archive.ubuntu.com/ubuntu/ edgy main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ edgy-security main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ edgy-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ edgy-proposed main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ edgy-backports main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ edgy main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ edgy-security main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ edgy-updates main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ edgy-proposed main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ edgy-backports main restricted universe multiverse
deb http://archive.ubuntu.org.cn/ubuntu-cn/ edgy main restricted universe multiverse

* Ubuntu.cn99.com 更新服务器(江苏省常州市电信，推荐电信用户使用。)：

deb http://ubuntu.cn99.com/ubuntu/ edgy main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ edgy-security main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ edgy-updates main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ edgy-proposed main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ edgy-backports main restricted universe multiverse
deb-src http://ubuntu.cn99.com/ubuntu/ edgy main restricted universe multiverse
deb-src http://ubuntu.cn99.com/ubuntu/ edgy-security main restricted universe multiverse
deb-src http://ubuntu.cn99.com/ubuntu/ edgy-updates main restricted universe multiverse
deb-src http://ubuntu.cn99.com/ubuntu/ edgy-proposed main restricted universe multiverse
deb-src http://ubuntu.cn99.com/ubuntu/ edgy-backports main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu-cn/ edgy main restricted universe multiverse

* Mirror.lupaworld.com 更新服务器(浙江省杭州市电信，亚洲地区官方更新服务器，推荐全国用户使用。)：

deb http://cn.archive.ubuntu.com/ubuntu edgy main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu edgy-security main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu edgy-updates main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu edgy-backports main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu edgy-proposed main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu edgy main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu edgy-security main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu edgy-updates main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu edgy-backports main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu edgy-proposed main restricted universe multiverse
deb http://mirror.lupaworld.com/ubuntu/ubuntu-cn edgy main restricted universe multiverse

* 上海市 上海交通大学 更新服务器(教育网，推荐校园网和网通用户使用。)：

deb http://ftp.sjtu.edu.cn/ubuntu/ edgy main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ edgy-backports main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ edgy-proposed main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ edgy-security main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ edgy-updates main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ edgy main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ edgy-backports main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ edgy-proposed main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ edgy-security main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ edgy-updates main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu-cn/ edgy main multiverse restricted universe

* 北京市清华大学 更新服务器(教育网，推荐校园网和网通用户使用。)：

deb http://mirror.net9.org/ubuntu/ edgy main multiverse restricted universe
deb http://mirror.net9.org/ubuntu/ edgy-backports main multiverse restricted universe
deb http://mirror.net9.org/ubuntu/ edgy-proposed main multiverse restricted universe
deb http://mirror.net9.org/ubuntu/ edgy-security main multiverse restricted universe
deb http://mirror.net9.org/ubuntu/ edgy-updates main multiverse restricted universe
deb-src http://mirror.net9.org/ubuntu/ edgy main multiverse restricted universe
deb-src http://mirror.net9.org/ubuntu/ edgy-backports main multiverse restricted universe
deb-src http://mirror.net9.org/ubuntu/ edgy-proposed main multiverse restricted universe
deb-src http://mirror.net9.org/ubuntu/ edgy-security main multiverse restricted universe
deb-src http://mirror.net9.org/ubuntu/ edgy-updates main multiverse restricted universe
deb http://mirror.net9.org/ubuntu-cn/ edgy main multiverse restricted universe

* 中国 台湾省台湾大学 更新服务器(推荐网通用户使用，电信PING平均响应速度41MS。)

deb http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy main restricted universe multiverse
deb-src http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy main restricted universe multiverse
deb http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy-updates main restricted universe multiverse
deb-src http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy-updates main restricted universe multiverse
deb http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy-backports main restricted universe multiverse
deb-src http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy-backports main restricted universe multiverse
deb http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy-security main restricted universe multiverse
deb-src http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy-security main restricted universe multiverse
deb http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy-proposed main multiverse restricted universe
deb-src http://ubuntu.csie.ntu.edu.tw/ubuntu/ edgy-proposed main restricted universe multiverse
deb http://ftp.sjtu.edu.cn/ubuntu-cn/ edgy main multiverse restricted universe

* Mirror.vmmatrix.net 更新服务器(上海市电信，推荐电信网通用户使用。):

deb http://mirror.vmmatrix.net/ubuntu/ edgy main restricted universe multiverse
deb-src http://mirror.vmmatrix.net/ubuntu/ edgy main restricted universe multiverse
deb http://mirror.vmmatrix.net/ubuntu/ edgy-updates main restricted universe multiverse
deb-src http://mirror.vmmatrix.net/ubuntu/ edgy-updates main restricted universe multiverse
deb http://mirror.vmmatrix.net/ubuntu/ edgy-backports main restricted universe multiverse
deb-src http://mirror.vmmatrix.net/ubuntu/ edgy-backports main restricted universe multiverse
deb http://mirror.vmmatrix.net/ubuntu/ edgy-security main restricted universe multiverse
deb-src http://mirror.vmmatrix.net/ubuntu/ edgy-security main restricted universe multiverse
deb http://mirror.vmmatrix.net/ubuntu/ edgy-proposed main multiverse restricted universe
deb-src http://mirror.vmmatrix.net/ubuntu/ edgy-proposed main restricted universe multiverse
deb http://ftp.sjtu.edu.cn/ubuntu-cn/ edgy main multiverse restricted universe

ubuntu.cnsite.org 更新服务器 (福建省福州市 电信):

deb http://ubuntu.cnsite.org/ubuntu/ edgy main restricted universe multiverse
deb-src http://ubuntu.cnsite.org/ubuntu/ edgy main restricted universe multiverse
deb http://ubuntu.cnsite.org/ubun
tu/ edgy-updates main restricted universe multiverse
deb-src http://ubuntu.cnsite.org/ubuntu/ edgy-updates main restricted universe multiverse
deb http://ubuntu.cnsite.org/ubuntu/ edgy-backports main restricted universe multiverse
deb-src http://ubuntu.cnsite.org/ubuntu/ edgy-backports main restricted universe multiverse
deb http://ubuntu.cnsite.org/ubuntu/ edgy-security main restricted universe multiverse
deb-src http://ubuntu.cnsite.org/ubuntu/ edgy-security main restricted universe multiverse
deb http://ubuntu.cnsite.org/ubuntu/ edgy-proposed main multiverse restricted universe
deb-src http://ubuntu.cnsite.org/ubuntu/ edgy-proposed main restricted universe multiverse
deb http://ubuntu.cnsite.org/ubuntu-cn/ edgy main multiverse restricted universe
