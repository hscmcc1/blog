---
author: hesicong
comments: true
date: 2013-01-13 14:52:45+00:00
layout: post
slug: a_li_yun_zhu_ji_mian_fei_kuo_rong
title: 阿里云主机免费扩容
wordpress_id: 3202
categories:
- 生活
---

我购买的阿里云的系统盘有20G，数据盘也有20G。为了让有限的空间发挥更多的作用，要想点办法才行。

最简单的莫过于透明压缩。Linux下支持透明压缩的文件系统不多，Ubuntu 12.04中，能支持的有btrfs和zfs。之前试了一下btrfs，压缩率虽然还行，但有一次更改了压缩选项后，就起不动了，最后只能所有文件丢失。ZFS在Linux下也有，试了一下，相当好用。在12.04中，无任何问题。

安装PPA，得自己研究一下哟！

```
https://launchpad.net/~zfs-native/+archive/stable
```

然后安装ubuntu-zfs

完成后重启一下

我是想让home目录压缩，home挂载到数据盘，所以先将home目录的数据拷贝到另外的地方。

然后，创建一个home目录

```
zpool create home /dev/xvdb
```

然后zfs set compression=gzip home，使用gzip要比默认的压缩率更高。之前的使用默认的压缩，压缩率只能到2.5x，用gzip可以到5.x，相当不错。

通过zfs get compressratio home可以看到压缩率

```
root@cloud:/home# zfs get compressratio home
NAME  PROPERTY       VALUE  SOURCE
home  compressratio  5.10x  -
```

如果是20G的盘，则相当于扩大到了100G的容量。

那么系统盘的空间也不能浪费了，我们先创建一个img文件，然后将创建一个pool，这样就可以使用连续的空间了。创建一个10G的文件

```
dd if=/dev/zero of=/disk.img bs=100M count=100
```

挂载到/dev/loop0

```
loset /disk.img /dev/loop0
```

然后

```
zpool create home/sysdisk /dev/loop0
```

当然，你也可以将/dev/loop0加入到现在的home下面，使得home的容量进一步扩大。但小心的是，一旦系统盘出现了问题，或者没办法需要回滚数据，则数据可能会丢失。

好吧，就到这里，大家试一下吧，很爽的哟~！
