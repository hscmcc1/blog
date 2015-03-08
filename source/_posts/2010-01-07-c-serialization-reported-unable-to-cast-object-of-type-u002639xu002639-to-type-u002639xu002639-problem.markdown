---
author: hesicong
comments: true
date: 2010-01-07 07:40:37+00:00
layout: post
slug: c-serialization-reported-unable-to-cast-object-of-type-u002639xu002639-to-type-u002639xu002639-problem
title: C#序列化报Unable to cast object of type 'X' to type 'X'的问题
wordpress_id: 2136
categories:
- 其他
tags:
- c
- cast
- serialize
- vs
---

最近遇到个奇怪的现象，在调试我的C#程序时，如果VS不打开“启动非托管代码调试”的话，序列化我的一个ApplicationConfig类就会报告“Unable to cast object of type 'X' to type 'X'”的问题，打开后万事大吉，但是无法进行暂停后编辑运行。

遇到这个问题后第一反应就是VS出问题了，重装系统后无法解决问题。

后GOOGLE到：http://www.google.cn/search?hl=zh-CN&source=hp&q=Can%27t+cast+type+X+to+X&btnG=Google+%E6%90%9C%E7%B4%A2&aq=f&oq=

发现其他人也遇到相同的问题，但具体问题不太一样。但大体看来是程序集出了问题。我选择清空项目然后重新编译，问题依旧，这就奇怪了。

最后偶然之间到我的编译目录下一看，清空后竟然dll文件还在。手动删除后，一切问题解决。

最后看来，还是DLL没有被正确删除造成的。那位什么没有被删除的DLL会影响随后的编译和调试呢？神奇了。
