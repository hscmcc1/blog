---
author: hesicong
comments: true
date: 2008-10-02 12:40:54+00:00
layout: post
slug: too-much-gcc-will-call-forwarding
title: 太强了……gcc也会呼叫转移～～
wordpress_id: 380
categories:
- 其他
tags:
- c
---

今天编译自己的代码，一句话：

```
fc->BoxSize=this->boxSize();
```

编译出错，gcc编译器说：

```
../VolumeRenderBox.cpp: In member function 'osg::ref_ptr<osg::Group> VirtualSurgery::VolumeRenderBox::createVolumeRenderBox(osg::Vec3, double)':
../VolumeRenderBox.cpp:218: 错误： 正在将呼叫转移到 ���A (无应答)
make: *** [VolumeRenderBox.o] 错误 1
```

太强了……发现gcc还可以呼叫转移，还无应答～～～真是绝了。

其实上一句话写错了，应该是

```
fc->BoxSize=this->boxSize;
```

整了一天的程序把成员变量都加括号了，呵呵。倒是这个编译提示真让人费解～～～
