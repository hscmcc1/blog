---
author: hesicong
comments: true
date: 2008-10-27 09:09:00+00:00
layout: post
slug: vmware-workstation-6-5-under-ubuntu-8-10-key-position-in-the-failure-of-problem-solving
title: VMWare workstation 6.5在Ubuntu 8.10下键位失灵问题解决
wordpress_id: 389
categories:
- 软件
tags:
- Linux
- vmware
---

今天把UBUNTU 8.10装好了，字体使用了文泉译的字体，很好看。哪知道装了虚拟机就来了很多问题。发现虚拟机里面方向键不能用了，其实回车键右边的所有键都不能用了。怎么回事哟。以前6.5在8.04上都好好的呢，怎么现在出了问题呢？6.5的BUG？和8.10不兼容？？在网上搜了N久没有找到问题的描述。

后来放弃了，装了VirtualBox，好用，但问题来了，虚拟Windows98需要关闭虚拟化技术～只能软件模拟～～哎，毕竟是16位和32位混合的产物，指令没法直接使用的。没法和Windows2000同使用，而且好像没有相应的显卡驱动，麻烦。试了试KVM，不支持98～放弃。

最后还是回到VMware来，怎么回事呢？？？？还亏了我巨大的耐心和无敌的搜索技能，终于[找到一个和我一样的人](http://communities.vmware.com/message/918459#918459)～～，一看问题和我一样，高兴了。哟，那儿去找~/.vmware/config文件哦，没有，郁闷了。嘿嘿，再看了看，文中提到了[“The Longer Story"](http://www.vmware.com/support/ws55/doc/ws_devices_keymap_linux_longer.html)，草草的看了看，明白了起中的道理，好的把/etc/vmware/config最后一行加入：

```
xkeymap.nokeycodeMap = true
```

就OK了。

我想，可能问题在于键位映射，和选择的中文环境可能有一些关系。
