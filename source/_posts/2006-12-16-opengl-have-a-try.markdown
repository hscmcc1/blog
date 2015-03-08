---
author: hesicong
comments: true
date: 2006-12-16 05:36:55+00:00
layout: post
slug: opengl-have-a-try
title: OpenGL Have a Try
wordpress_id: 106
categories:
- 其他
tags:
- opengl
---


看了一个多星期的OpenGL的书，对OpenGL总算有一个粗浅的了解，昨天和今天开始进行实战。

首先从最基本的画一些图形开始，这个很简单。

然后就是视角的转换，这个稍微有些麻烦，很多时候辛辛苦苦做了一个程序，结果什么都看不到，就是视角没有弄好。好好的看看书问题就解决了。做好视角的基础以后就继续做可以动的视角，期间发现glutSetWindowTitle的函数执行会影响opengl程序运行，所以把所有的字都打印在了窗体里面。终于，视角处于一个球坐标系里面，可以任意移动。

然后就是开始尝试着色颜色、灯光、隐面消除、Alpha混合。隐面消除的时候于到了一些问题，glut库里面的teapot和其他的图形的三角形的绕向是相反的，所以只好做一个转换。另外，gluPerspective的zNear参数如果设置为0的话，DepthTest会出现问题(如下图)。呵呵，最初我就是这样设置，结果我找了好久的问题，最后还是不断的比较找到了：)

正确的DepthTest:
[](/images/others/320061215223336.png)![](/images/others/image/thumb/320061215223336.png)

错误的DepthTest:
[](/images/others/520061215223355.png)![](/images/others/image/thumb/520061215223355.png)

最后就是添加一些好玩的元素进去，包括可爱的雾化效果：)

程序的部分执行效果，可以实时拉近、拉远、设置雾的远近等：
[](/images/others/520061215223513.png)![](/images/others/image/thumb/520061215223513.png)
[](/images/others/u20061215223521.png)![](/images/others/image/thumb/u20061215223521.png)

最后，源代码保留一个，然后看看Nvidia SDK，里面有好多好多的特效阿～～～：)

[download id="20"]
