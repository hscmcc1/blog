---
author: hesicong
comments: true
date: 2007-09-27 22:56:58+00:00
layout: post
slug: try-windows-server-2008
title: 试用Windows Server 2008
wordpress_id: 249
categories:
- 软件
---


昨天晚上终于把Windows Server 2008 RC0下载完毕了，第一件事就是腾出空间安装。

第一道题就把我郁闷惨了，SATA硬盘怎么分区？我用过很多老牌的，加上现在最新的分区软件，统统都被SATA硬盘挣郁闷了。所有的现象就是，找不到硬盘。要知道很多分区软件对IDE支持很好，由于SATA的独特性，导致这些分区软件纷纷下马。最后只得在XP下面进行无损分区了，装好Acronis Disk Director Suite 10，很完美的就成功了。当然绝对不要使用Windows自带的分区软件，这玩意儿害人不浅，很有可能你的数据就在一念之间灰飞烟灭。要找回来那是相当的困难啊……

第二道题就简单了，驱动问题，显卡、主板、声卡驱动很顺利的装上了，唯独那个ADMtek AM983网卡没有驱动。哎，当时买网卡的时候就贪图便宜嘛，没有买8139的，这下这个网卡VISTA下面的驱动都没有。网上搜寻了一阵，大家说用XP的驱动就对了，试试，果然对了，呵呵。好用！

刚装上的系统什么都没有，与Vista不一样，2008需要安装Desktop Experience并打开Theme这个服务，而且要将配色方案里面的Aero Basic改成Aero才能看得到Aero效果。说实在的，自从用了Linux下面的XGL衍生的3D桌面技术以后，对于Vista的效果只能嗤之以鼻了。当然，我的显卡在Vista下面也不能完美的调速，Shader频率始终不能降下来，导致GPU温度一直保持在60度以上，比XP的53度高了不少。相比其功耗也要大很多，浪费啊……

2008的启动比Vista的拖拉好多了，很迅速，整个环境也很简洁。当然2008兼容性还存在一定问题，第一个装的迅雷5就完蛋了，一打开就报错，害得我只有安装WEB迅雷。World In Conflict也玩不了，倒是尘埃还可以玩。NFS9就不说了，直接报错。其他应用程序没有看出来什么太大的问题。

后来为了玩一玩看一看World In Conflict的画面，再另一个分区装了个Vista，结果后来切换回2008的时候竟然无法登陆了。安全模式进去发现原来的K盘变成了D盘，导致找不到相关的文件。算了算了，懒得用了，毕竟2008的兼容性问题还不能成为主流，毕竟还是一个测试版本，最后的决定，2008和Vista一起删除了……

这下准备安装Ubuntu 7.10 Alpha 4，平时要用windows就用虚拟机吧，分1.5G内存给他应该够用了，实在不够分2G，反正Linux平时也用不到太多内存。虚拟机分2个核心给他吧，给2个给他用就是。还可以开一个虚拟机，呵呵……算了，太疯狂了。

睡觉~
