---
author: hesicong
comments: true
date: 2009-10-03 08:12:42+00:00
layout: post
slug: openembedded-console-image-build-success
title: OpenEmbedded console-image 编译成功
wordpress_id: 2030
categories:
- 硬件
- 软件
tags:
- console-image
- mini2440
- openembedded
---

我是才接触OpenEmbedded不久，也算是个菜鸟。看到这么方便的东西以为bitbake xxxx就可以了，结果哪知道这个才是唐僧刚刚上马，遇到的妖魔鬼怪还多着呢。这里总结一下我最近bitbake console-image所历经的磨难。

[](/images/mini2440/IMG_0117.JPG)![](/images/mini2440/image/thumb/IMG_0117.JPG)

[](/images/mini2440/IMG_0118.JPG)![](/images/mini2440/image/thumb/IMG_0118.JPG)

[](/images/mini2440/IMG_0119.JPG)![](/images/mini2440/image/thumb/IMG_0119.JPG)

. 准备

* 一个超级好的网络是必不可少的，否则当你什么都下载不了的时候你就知道什么是痛苦了。
* 一个4核的CPU是很有必要的，当然如果有钱可以买个core i7更好
* 如果想在编译的时候能够打发一下时光，建议安装vmware。特别建议用7.0的技术预览版(网上找找看有没有，我是vmware邀请测试的)，因为可以支持大于2个CPU。
* 留至少80G连续的硬盘空间。
* 选一个比较吉祥的日子，准备好耐心
. 下载和建立好local.conf

从OE的官方git源clone一个副本，这个步骤我就省略了，但是记得经常`git pull`。我的local.conf的MACHINE定义为mini2440，DISTRO定义为openmoko。想编译一个openmoko出来玩。

```
bitbake console-image
```
然后bitbake就会工作了，期间肯定会遇上小的妖魔鬼怪，什么preferred version not found之类的，只需要改变一下conf/distro/conf文件里面的preferredxxxxx.inc文件就可以了。具体是什么文件可以用`grep xxxx -r .`来搜索，当然这你得有一些基本的linux尝试。

其他的问题比较多的是：

* 编译出错。我想这不是我的错，如果在recipes的相应组件中找到了更新的版本，可以使用另外的版本来替换。替换的方法是修改PREFERRED_VERSION_xxxxx的值即可。一般使用高点的版本就会OK。
* 没有本地的工具支持。有些编译项目需要本地支持。上次编译mtd-utils就遇到了本地有一个工具没有安装。如果看到了有些错误位于i686目录(有这个字样的)，就可以怀疑是本地有些工具或者库没有安装，从而不能正常编译。懂点脑子把东西装上应该就会对。
* 版本不兼容。有时候包与包之间有可能不兼容，方法是把出错的包在网上搜一下，例如这个编译mtd-native的错误就是由包之间不兼容引起的：http://projects.linuxtogo.org/pipermail/openembedded-devel/2009-January/007571.html
* 其它问题。有时候编译中途出了问题(例如电脑死机、意外重启、停电等)会造成一些莫名其妙的文傳統，把tmp文件夹删除了再编译，看会不会OK。当然删除tmp文件夹要有点耐心，文件太多了。

如果想在晚上通宵运行，那么加一个`-k`参数就好，这样第二天早上再来处理那些莫名其妙的问题。

写这篇文章的时候我只记得这么几个问题了，还有一些另外的小问题，但也都是很容易解决的。多多搜索，或者利用邮件列表来解决。

现在开始另一站了，编译`bitbake x11-office-image`。

外面下雨了，这个国庆真是恼火~
