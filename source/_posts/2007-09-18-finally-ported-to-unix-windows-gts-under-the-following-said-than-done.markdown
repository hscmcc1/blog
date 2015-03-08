---
author: hesicong
comments: true
date: 2007-09-18 19:18:34+00:00
layout: post
slug: finally-ported-to-unix-windows-gts-under-the-following-said-than-done
title: 终于把UNIX下的GTS移植到WINDOWS下面~难啊
wordpress_id: 245
categories:
- 其他
tags:
- gts
---

最近一致在寻找一个很好的几何实体布尔操作的库，找到了小巧精致但特别功能要收费的的sgCore，强大的不知道怎么使用的openCASCADE，GNU开源UNIX系统的GNU Triangulated Surface Library(GTS)，还听闻一些收费的HOOPS等。

最开始，sgCore非常让我满意，精巧的结构，很易于使用的编程风格，强大的功能，小巧的身材……可是，当需要用到将三角形模型转换成实体模型的时候，就要收费了。最低收费标准400美元，我的天啊，抢人……

最终还是放弃了这个美丽的“陷阱”。

openCASCADE库，借用论坛上坐沙发流行的一句话：很好，很强大！无与伦比的功能，包括CAD/CAM的方方面面，从二维样条曲线，到三维的实体操作，样样俱全，可是，太大了……600多M的安装包，加上200M的文档，源程序还是*.cxx的。强大到根本搞不清楚怎么入手，最终放弃了…………

最后的希望来自于GNU的GTS，小巧，免费，精致，强大。唯独一个缺点是目前只有UNIX版本的，虽然写了可以在WIN32下面编译，但是其MAKEFILE还是VC6时代的，还需要3个依赖包。而这两个做好的WIN32下的依赖包还是VC6编译的，即便在VC++2005下面吧GTS编译成功了，也用不起。因为这两个DLL用的是MSVCRT.DLL，而VC++2005编译的GTS库用的MSVCRT80.DLL，会导致不兼容。

查阅了大量的资料，发现解决方法有如下：

* 使用cygwin编译。缺点很显然，我写程序也得在cygwin下面去编译，显然有些不太方便。而且编译出来的DLL还不好用。
* 用VC++2005全部重新编译所有的依赖包。
* 放弃……~

最终选择了2。拼死活命也要将GTS编译出来。

第一个难题就是glib的编译问题。因为GTS需要用到glib，所以就到网上寻glib的win32版本。竟然，还是只有那个VC6的版本。还好，glib最新的源代码中已经包含了VC++2005的makefile了。还需要找gettext和libiconv的WIN32版本。

经过查询，找到gettext 0.14.4，据说可以编译成功。试了N久，发现少了relocate.h这个头文件，即使弄上去了编译也出问题。又是google，找到一个人的问题和我一样，解决方案是用0.14.6版本编译。果然通过了。幸福……

编译libiconv也遇到了一些莫名其妙的问题，都是一些什么玩意儿没有定义啊，什么宏没有定义这些，很烦人。glib也是一些win32下老的makefile需要更新。……

4天时间都在搞这些玩意儿，彻底记不清楚具体是怎么把他搞成功的了。最后做了一个安装包，只需执行一个批处理，傻瓜化的就完成了编译安装。……

PS：准备又要开始研究SMS相关的东西了……哎，一天忙啊

下载：

http://www.hesicong.net/Store/gts_win32_build_vc8.rar

包含所有源代码，在VS2005的命令提示里面直接执行build_all.bat即可编译安装成功，很费功夫的哦：

http://www.hesicong.net/Store/gts_win32_src.rar
