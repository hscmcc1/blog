---
author: hesicong
comments: true
date: 2008-05-27 04:49:12+00:00
layout: post
slug: how-to-get-the-object-world-coordinate-shader
title: 如何用Shader得到物体的世界坐标
wordpress_id: 360
categories:
- 其他
tags:
- opengl
- OSG
---

最近群里面有个朋友问我关于如何得到OpenGL世界坐标的问题，当时我还弄错了，误以为`gl_ModelViewMatrix*gl_Vertex`就是世界坐标。因最近也突然遇到了世界坐标的问题，所以花了一些时间来研究这个问题，网上也有人问，但或许没有答案，或许是错的。
其实，OpenGL的转换管道直接将gl_Vertex，也就是物体坐标，用gl_ModelViewMatrix相乘，得到的是眼坐标。如果将gl_ModelViewMatirx拆分为gl_ModelMatrix和gl_ViewMatrix，那么问题就好解决了。但事实上没有提供。要清楚OpenGL其实没有世界坐标系，世界坐标系是应用程序的概念。其实可以将OpenGL的摄像机看作是固定的，其坐标系就是眼坐标系，移动摄像机和移动物体的位置是一个相反的转换，对于观察者来说根本不知道是摄像机在动，还是物体在动(想想大卫的大变自由女神像的魔术吧，呵呵)
说回来，最终的变换是这样的：

```
eyePos=viewMatrix * modelMatrix * modelVertex
```

在OpenGL里面viewMatrix和modelMatrix合并了，因为OpenGL里面并没有设置摄像机的参数，所以OpenGL并不知道viewMatrix到底是什么。viewMatrix是用户自己定义的，所以如果能够得到这个viewMatrix并能得到其逆矩阵，就可以得到worldPos:

```
worldPos=viewMatrixInv * viewMatrix * modelMatrix * modelVertex
```

传统的OpenGL程序里面，你得自己计算这个viewMatrixInv，还好OSG的Camera提供了一个getViewMatrixInverse()方法，通过这个方法我们就可以轻松的获得viewMatrixInv，然后传递给Vertex Shader(用一个Uniform就可以)，然后进行这个计算就可以了。
记得每一帧都需要Update这个viewMatrixInv，只需要一个updateCallBack就可以了。
好了，看几个图，我用3DSMAX创建了两个盒子，为了便于观察，模型的顶点值限制在0-1之间，然后用osgExp导出，没有选中Flatten Static Transform这样就不会把模型定点转换成世界坐标系的顶点。
源代码中可以改变gl_FragColor=的值来修改为相应的坐标系的值显示。
世界坐标系的最终输出，可见颜色连续变换的。

[](/images/others/world.jpg)![](/images/others/image/thumb/world.jpg)
眼坐标系的图，可见屏幕中间偏左的部分是黑的。因为其值是负的。平移拖动盒子可见相应像素着色不变。

[](/images/others/eye.jpg)![](/images/others/image/thumb/eye.jpg)

物体坐标系的图，可见两个盒子的颜色一样，因为其值是相同的。
[](/images/others/local.jpg)![](/images/others/image/thumb/local.jpg)
源代码也附后，可以运行着看看，虽然程序简单，但用到的时候再也不用苦苦思考了，呵呵。

[[download id="5"]
](http://www.hesicong.net/blog/upload/2008/5/RenderWorldCoordinate.tar)
