---
author: hesicong
comments: true
date: 2007-04-03 06:08:10+00:00
layout: post
slug: openscenegraph-notes-form-mode
title: OpenSceneGraph 笔记--窗体模式运行
wordpress_id: 174
categories:
- 其他
tags:
- OpenSceneGraph
- OSG
---

1.使用osgViewer::Viewer代替原来的osgProducer::Viewer
2.先熟悉设计模式，比如最常用的Visitor设计模式。要不然看不懂程序不说，而且更写不出程序。
3.窗体模式运行参考Example osgWindows，在这个例子中重点在于：

```
osg::ref_ptr traits = new osg::GraphicsContext::Traits;
traits->x = 640;
traits->y = 0;
traits->width = 640;
traits->height = 480;
traits->windowDecoration = true;
traits->doubleBuffer = true;
traits->sharedContext = 0;

osg::ref_ptr gc = osg::GraphicsContext::createGraphicsContext(traits.get());

osg::ref_ptr camera = new osg::Camera;
camera->setGraphicsContext(gc.get());
camera->setViewport(new osg::Viewport(0,0, traits->width, traits->height));
GLenum buffer = traits->doubleBuffer ? GL_BACK : GL_FRONT;
camera->setDrawBuffer(buffer);
camera->setReadBuffer(buffer);

// add this slave camra to the viewer, with a shift right of the projection matrix
viewer.addSlave(camera.get(), osg::Matrixd::translate(-1.0,0.0,0.0), osg::Matrixd());
```

另外Example osgkeyboardmouse也提到了另外一种方式：

```
// create the window to draw to.
osg::ref_ptr traits = new osg::GraphicsContext::Traits;
traits->x = 200;
traits->y = 200;
traits->width = 800;
traits->height = 600;
traits->windowDecoration = true;
traits->doubleBuffer = true;
traits->sharedContext = 0;

osg::ref_ptr gc = osg::GraphicsContext::createGraphicsContext(traits.get());
osgViewer::GraphicsWindow* gw = dynamic_cast(gc.get());
if (!gw)
{
osg::notify(osg::NOTICE)<<"Error: unable to create graphics window."<
return 1;
}

gw->realize();
gw->makeCurrent();

// create the view of the scene.
osgViewer::SimpleViewer viewer;
viewer.setSceneData(loadedModel.get());

viewer.setEventQueue(gw->getEventQueue());
viewer.getEventQueue()->windowResize(traits->x,traits->y,traits->width,traits->height);
```

TODO:
1.3DS导入插件还不够完善，需要自己写。
2.更深入的了解OSG的机制。
