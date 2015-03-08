---
author: hesicong
comments: true
date: 2008-12-28 11:20:45+00:00
layout: post
slug: linux-using-wine-successfully-simulate-the-following-tm2008-beta
title: Linux下面成功使用Wine模拟TM2008 Beta
wordpress_id: 1507
categories:
- 软件
tags:
- Linux
- ubuntu
- wine
---

零点今天给我说他模拟成功了TM2008，哇，真的啊？他给了我这个网址：[Linux 下使用 Wine 安装运行 TM2008 Beta 及乱码、与 Windows 共享聊天记录等相关问题的解决](http://linuxtoy.org/archives/running-tm2008-beta-with-wine.html)

按照网上给的方法稍作修改以后，安装成功了。

首先是把以前的wine卸载了。安装了[打了中文补丁的wine 1.13](http://forum.ubuntu.org.cn/viewtopic.php?f=121&t=103958)，就访问http://ftp.ubuntu.org.cn/home/windowssux/Wine_CN/下载并安装最新的wine1.13就可以解决中文问题了。

然后拷贝[rpcrt4.dll](http://www.rainux.org/stuff/rpcrt4.dll.gz)到wine的system32目录，然后

```
sh winetricks msxml3 gdiplus riched20 riched30 ie6 vcrun6 vcrun2005sp1
```

安装一怕啦东西。

最后就是执行TM的安装程序就可以啦。

零点装的是TM Preview 4，貌似字体比较小。我这里装的是TM Beta，但是无法上线，按了上线几秒钟就离开了，郁闷的很。

但无论杂说，还是基本可用了，比用QQ几分钟挂一次好多了，比用QQ FOR LINUX好太太太多了～～感谢wine
