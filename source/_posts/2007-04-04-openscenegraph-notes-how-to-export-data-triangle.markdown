---
author: hesicong
comments: true
date: 2007-04-04 22:59:34+00:00
layout: post
slug: openscenegraph-notes-how-to-export-data-triangle
title: OpenSceneGraph 笔记--如何导出三角形数据
wordpress_id: 176
categories:
- 其他
tags:
- OpenSceneGraph
---

在OpenSceneGraph开发中，为了方便会经常使用到一些不是三角形片的数据，比如四边形等数据。例如画一个管子用四边形带比用三角形片好计算得多。比如现在我们要画一个由两个平面组成的面，我可以这样做：

``` cpp
osg::Geode* geode=new osg::Geode;
osg::Geometry* polyGeom = new osg::Geometry;
osg::Vec3 myCoords[]=
{
osg::Vec3(0,1,0),
osg::Vec3(0,0,0),
osg::Vec3(1,1,0),
osg::Vec3(1,0,0),
osg::Vec3(2,1,0),
osg::Vec3(2,0,0)
};

int numCoords = sizeof(myCoords)/sizeof(osg::Vec3);
osg::Vec3Array* vertices = new osg::Vec3Array(numCoords,myCoords);
polyGeom->setVertexArray(vertices);
polyGeom->addPrimitiveSet(new osg::DrawArrays(osg::PrimitiveSet::QUAD_STRIP,0,numCoords));
geode->addDrawable(polyGeom);
```

这样就用6个点，用OpenGL提供的QUAD_STRIP方式画出了两个平面。
但是如果要把这个平面用于碰撞检测等技术，那么就需要把这六个点所表示的四边形带转换成三角形片才行。这些三角形定点如下：

```
0 1 0
0 0 0
1 1 0

0 0 0
1 0 0
1 1 0

1 1 0
1 0 0
2 1 0

1 0 0
2 0 0
2 1 0
```

可以看出两个平面由4个三角形组成，而且都是逆时针排列(朝向一致)。
以前我自己做过转换，但是感觉很麻烦。OpenSceneGraph的Example osggeometry中提供了一个printTriangles函数，它可以打印出一个drawable所有的三角形片，不管最初的数据结构如何：

``` cpp
struct NormalPrint
{
void operator() (const osg::Vec3& v1,const osg::Vec3& v2,const osg::Vec3& v3, bool) const
{
osg::Vec3 normal = (v2-v1)^(v3-v2);
normal.normalize();
std::cout << "t("<<<") ("<<<") ("<<<") "<<") normal ("<<<")"<
}
};

// decompose Drawable primtives into triangles, print out these triangles and computed normals.
void printTriangles(const std::string& name, osg::Drawable& drawable)
{
std::cout<<

osg::TriangleFunctor tf;
drawable.accept(tf);

std::cout<
}
```

核心的思想就是利用osg::TriangleFunctor这个模版。这个模版会让你重载()运算符，然后让Drawable去visit它。在这个过程中，所有原始的数据(不管是三角形片的，还是四边形的)都转换成了三角形片数据。

那么如何把三角形数据导出哪？只需要修改一下借助这个思路，将NormalPrint修改成我们需要的就对了。

``` cpp
struct GetVertex
{
void operator() (const osg::Vec3& v1,const osg::Vec3& v2,const osg::Vec3& v3, bool) const
{
vertexList->push_back(v1);
vertexList->push_back(v2);
vertexList->push_back(v3);
}

osg::Vec3Array* vertexList;

};

void getTriangles(osg::Drawable& drawable)
{
osg::TriangleFunctor tf;
tf.vertexList=new osg::Vec3Array;

drawable.accept(tf);

for(osg::Vec3Array::iterator itr=tf.vertexList->begin();
itr!=tf.vertexList->end();
itr++)
{
osg::Vec3 vertex=*itr;
std::cout<<
}

std::cout<
}
```

以下是完整的示例文件：

``` cpp
// PrimitiveSet.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include

struct GetVertex
{
void operator() (const osg::Vec3& v1,const osg::Vec3& v2,const osg::Vec3& v3, bool) const
{
vertexList->push_back(v1);
vertexList->push_back(v2);
vertexList->push_back(v3);
}

osg::Vec3Array* vertexList;

};

void getTriangles(osg::Drawable& drawable)
{
osg::TriangleFunctor tf;
tf.vertexList=new osg::Vec3Array;

drawable.accept(tf);

for(osg::Vec3Array::iterator itr=tf.vertexList->begin();
itr!=tf.vertexList->end();
itr++)
{
osg::Vec3 vertex=*itr;
std::cout<<
}

std::cout<
}

osg::Node* createGeode()
{
osg::Geode* geode=new osg::Geode;
osg::Geometry* polyGeom = new osg::Geometry;
osg::Vec3 myCoords[]=
{
osg::Vec3(0,1,0),
osg::Vec3(0,0,0),
osg::Vec3(1,1,0),
osg::Vec3(1,0,0),
osg::Vec3(2,1,0),
osg::Vec3(2,0,0)
};

int numCoords = sizeof(myCoords)/sizeof(osg::Vec3);
osg::Vec3Array* vertices = new osg::Vec3Array(numCoords,myCoords);
polyGeom->setVertexArray(vertices);
polyGeom->addPrimitiveSet(new osg::DrawArrays(osg::PrimitiveSet::QUAD_STRIP,0,numCoords));
geode->addDrawable(polyGeom);
getTriangles(*polyGeom);
return geode;
}

int _tmain(int argc, _TCHAR* argv[])
{
//Set up viewer
osgViewer::Viewer viewer;
osg::ref_ptr traits=new osg::GraphicsContext::Traits;
traits->x=200;
traits->y=200;
traits->width=800;
traits->height=600;
traits->windowDecoration=true;
traits->doubleBuffer=true;
traits->sharedContext=0;

osg::ref_ptr gc=osg::GraphicsContext::createGraphicsContext(traits.get());
osg::ref_ptr camera=new osg::Camera;
//osg::Camera camera=new osg::Camera;
camera->setGraphicsContext(gc.get());
camera->setViewport(new osg::Viewport(0,0,traits->width,traits->height));
camera->setDrawBuffer(GL_BACK);
camera->setReadBuffer(GL_BACK);
osgGA::TrackballManipulator* tm=new osgGA::TrackballManipulator;

viewer.setCameraManipulator(tm);

viewer.addSlave(camera.get());

//Set up root node
osg::ref_ptr root=new osg::Group;

root->addChild(createGeode());

//Start show!
viewer.setSceneData(root.get());
viewer.realize();

while(!viewer.done())
{
viewer.frame();
}
}
```

