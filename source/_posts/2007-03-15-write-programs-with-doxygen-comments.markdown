---
author: hesicong
comments: true
date: 2007-03-15 05:12:21+00:00
layout: post
slug: write-programs-with-doxygen-comments
title: 用doxygen写程序注释
wordpress_id: 155
categories:
- 其他
tags:
- doxygen
---

接触doxygen是从接触lib3ds库开始的，这个库的源代码使用doxygen作为文档的注释，很爽，直接使用工具就可以生成一个非常漂亮的文档。所以我也开始学着用doxygen作注释。

很简单，一个文件的开始写文档的头

```
/*! file: filename
brief: This is a file
author: hesicong
.....
*/
```

对于类

```
//! Class Description
```
对于函数

```
/*! Function
parm parm1 description1
parm parm2 description2
*/
```
等等，感觉很简单的东西，就可以生成很强大的文档，包括类的继承关系、函数的调用关系都很清楚～～～

唉，说了半天，自己去尝试一下～～～～

另外，一直用Subversion管理源代码，很爽～！
