---
author: hesicong
comments: true
date: 2005-01-21 15:24:52+00:00
layout: post
slug: practical-tips-fast-decoding-unicodeutf8-coding
title: 实用小技巧——迅速解码UNICODE/UTF8编码
wordpress_id: 1186
categories:
- 其他
---


编程难免遇到需要转换Unicode或UTF8到字符串的情形。例如在vCard里面就有

```
X-ESI-CATEGORIES;CHARSET=UTF-8;ENCODING=QUOTED-PRINTABLE:=E6=9C=AA=E8=AE=BE=
=E5=AE=9A=E7=BE=A4=E7=BB=84
```

我们关注这一句的后面部分，使用的是UTF8编码。我想知道它包含的是什么内容，又不想编程，我们可以借用Winhex编辑一个文本文件，然后用Notepad(记事本)打开，就可以知道具体内容了。

例如上面的UTF8字节：E6 9C AA E8 AE BE E5 AE 9A E7 BE A4 E7 BB 84 共15个字节

我们在打开Winhex，创建一个15字节的空文件，然后输入以上字节。


然后保存为一个文本文件，此例用Test.txt。


然后打开Test.txt，可以看到中文：未设定群组


这样，就实现了简单的解码功能。
