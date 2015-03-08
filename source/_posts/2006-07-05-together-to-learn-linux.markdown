---
author: hesicong
comments: true
date: 2006-07-05 10:17:15+00:00
layout: post
slug: together-to-learn-linux
title: 一起来学习Linux
wordpress_id: 1448
categories:
- 软件
---


开源社区Lupa给广大Linux初学者提供了一个实验室，通过SSH远程连接到lab.lupa.cn服务器，然后就可以学习一些Linux基本的命令。


服务器信息

登陆地址： lab.lupa.cn

用户名：   lab

密码：   lab

推荐登陆器下载
[SSHSecureShellClient-3.2.9.exe ](http://www.lupa.gov.cn/down/SSHSecureShellClient-3.2.9.exe)


我登陆上去看了看

用的是赛扬的1G的CPU，还有128MB的内存，跟我家里面的有个便宜货一样，赫赫：)
[lab@lab:/proc$](mailto:lab@lab:/proc$) cat cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 11
model name      : Intel(R) Celeron(TM) CPU                1000MHz
stepping        : 4
cpu MHz         : 1002.463
cache size      : 256 KB
fdiv_bug        : no
hlt_bug         : no
f00f_bug        : no
coma_bug        : no
fpu             : yes
fpu_exception   : yes
cpuid level     : 2
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 mmx fxsr sse
bogomips        : 1986.56

[lab@lab:/proc$](mailto:lab@lab:/proc$) cat meminfo
MemTotal:       126904 kB
MemFree:         88928 kB
Buffers:          9352 kB
Cached:          14216 kB
SwapCached:          0 kB
Active:          19784 kB
Inactive:         7552 kB
HighTotal:           0 kB
HighFree:            0 kB
LowTotal:       126904 kB
LowFree:         88928 kB
SwapTotal:      369452 kB
SwapFree:       369452 kB
Dirty:               0 kB
Writeback:           0 kB
Mapped:           6464 kB
Slab:             6392 kB
Committed_AS:    46672 kB
PageTables:        260 kB
VmallocTotal:   901112 kB
VmallocUsed:      3792 kB
VmallocChunk:   896876 kB
