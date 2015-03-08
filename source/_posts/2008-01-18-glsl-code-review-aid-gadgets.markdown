---
author: hesicong
comments: true
date: 2008-01-18 03:16:35+00:00
layout: post
slug: glsl-code-review-aid-gadgets
title: glsl代码检查辅助小工具
wordpress_id: 301
categories:
- 其他
---

osg里面的是用glsl写shader的，对于shader貌似osg只会在第一次要用到的时候才编译shader，对于有问题的shader也只能控制台输出错误，不太方便调试。找了一圈没有找到小工具能够编译一下glsl然后报告有没有语法错误之类的，所以自己写了一个，源代码和可执行文件附后。使用方法：
glslChecker vs
glslChecker fg
控制台就会输出warning或者error。
程序写的简单，难免有问题，算是抛砖引玉，如果有朋友用过类似的更好的工具，还望多多交流。

[download id="9"]
