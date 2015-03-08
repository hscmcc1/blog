---
author: hesicong
comments: true
date: 2012-11-10 04:06:40+00:00
layout: post
slug: a_li_yun_diao_zheng_xi_tong_fen_qu_da_xiao
title: 阿里云调整系统分区大小
wordpress_id: 3181
categories:
- 其他
---

阿里云的云服务器，40G的硬盘，20G分给了系统，20G分给了数据。那么，如果不加调整，只能用上20G的空间，其余系统占用的20G就不怎么好用了。

那么，有没有办法能够将系统的20G分区分出来呢？有。resize2fs不支持挂在的分区的调整，我们需要将整个root转移到data盘，然后调整fstab和grub，让/挂在到/dev/xvdb1分区中，然后再对/dev/xvda进行调整，然后再将数据恢复回去。

. 备份数据

给data盘

```
fdisk /dev/xvdb
```

格式化

```
mkfs.ext4 /dev/xvdb1
```

挂载data盘到/mnt

```
mount /dev/xvdb1 /mnt
```

搬移整个root

```
rsync -aAXv /* /mnt --exclude={/dev/*,/proc/*,/sys/*,/tmp/*,/run/*,/mnt/*,/media/*,/lost+found,/home/*/.gvfs}
```

. 修改fstab和grub.cfg

首先查看GUID编号：

```
ls -l /dev/disk/by-uuid/
total 0
lrwxrwxrwx 1 root root 11 Nov 10 11:41 56a7fe0d-1d1c-4aa5-82ad-59dedb0177b3 -> ../../xvda1
lrwxrwxrwx 1 root root 11 Nov 10 11:42 cd6331ee-47db-4338-9288-82d66e7e1572 -> ../../xvdb1
```

看到xvda1的号和xvdb1的号，将在fstab中的56a7fe0d-1d1c-4aa5-82ad-59dedb0177b3替换成cd6331ee-47db-4338-9288-82d66e7e1572即可(每个空间的号不一样哈，不要乱替换)

替换好后，

```
reboot
```

如果一切顺利，1分钟之内则可以重新连上。否则只有重置阿里云了……

登录上去，则可以看到：

```
# mount
/dev/xvdb1 on / type ext4 (rw,errors=remount-ro)
proc on /proc type proc (rw,noexec,nosuid,nodev)
sysfs on /sys type sysfs (rw,noexec,nosuid,nodev)
none on /sys/fs/fuse/connections type fusectl (rw)
none on /sys/kernel/debug type debugfs (rw)
none on /sys/kernel/security type securityfs (rw)
udev on /dev type devtmpfs (rw,mode=0755)
devpts on /dev/pts type devpts (rw,noexec,nosuid,gid=5,mode=0620)
tmpfs on /run type tmpfs (rw,noexec,nosuid,size=10%,mode=0755)
none on /run/lock type tmpfs (rw,noexec,nosuid,nodev,size=5242880)
none on /run/shm type tmpfs (rw,nosuid,nodev)
```

xvdb1分区被挂载了。

好了。这下可以分区了。

. 重新分区

重新分区要把以前的xvda1分区删除了。如果想无损分区，似乎可以用resize2fs来做。但我采用的方法是将以前的分区删除了，然后重新将现有的系统拷贝过去。

```
fdisk /dev/xvda
```

删除第一个分区，然后新建一个分区，分区大小自己定。

记得按a添加一个boot flag，否则启动不了。然后p以下，应该可以看到有一个*

```
Device Boot      Start         End      Blocks   Id  System
/dev/xvda1   *        2048    10487807     5242880   83  Linux
```

然后

```
mkfs.ext4 /dev/xvda1
```

格式化

完成后，

```
mount /dev/xvda1 /mnt
```

然后，我们又要将现有系统同步过去：

```
rsync -aAXv /* /mnt --exclude={/dev/*,/proc/*,/sys/*,/tmp/*,/run/*,/mnt/*,/media/*,/lost+found,/home/*/.gvfs}
```

. 修改分区挂载。

由于重新启动后的分区将是/mnt下面的分区，那么，我们需要修改/mnt/etc/fstab和/mnt/boot/grub/grub.cfg内的挂在点。

修改方法和之前的一样，先

```
ls /dev/disk/by-uuid看一下GUID，然后再修改即可。
```

. 重启后，看一下分区信息，搞定。

```
df -h

Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1      5.0G  1.2G  3.7G  24% /
udev            237M  4.0K  237M   1% /dev
tmpfs            99M  264K   98M   1% /run
none            5.0M     0  5.0M   0% /run/lock
none            246M     0  246M   0% /run/shm
```