---
author: hesicong
comments: true
date: 2007-04-06 15:24:56+00:00
layout: post
slug: openscenegraph-notes-how-to-export-data-2-triangles
title: OpenSceneGraph 笔记--如何导出三角形数据2
wordpress_id: 178
categories:
- 其他
tags:
- OpenSceneGraph
- OSG
---

今天写了一个导出三角形的类,可以导出一个Group的所有三角形数据(包括Group的所有child),主要用于碰撞检测.比如有一个Group"自行车",这个Group包含有子Group"前轮"和子Group"后轮",子对象通过MatrixTransform与父对象相连.那么这个类可以将Group"自行车"包括"前轮"和"后轮"的三角形数据都导出到一个vector里面,方面用于碰撞检测.

代码片段如下:

```
struct GetVertex
{
void operator() (const osg::Vec3& v1,const osg::Vec3& v2,const osg::Vec3& v3, bool) const
{
osg::Vec3 v1New=v1*(*matrix);
osg::Vec3 v2New=v2*(*matrix);
osg::Vec3 v3New=v3*(*matrix);
vertexList->push_back(v1New);
vertexList->push_back(v2New);
vertexList->push_back(v3New);
}
osg::Vec3Array* vertexList;
osg::Matrix* matrix;
};

class VertexVisitor:
public osg::NodeVisitor
{
public:
VertexVisitor():osg::NodeVisitor(osg::NodeVisitor::TRAVERSE_ALL_CHILDREN){};

virtual void apply(osg::Geode& geode)
{
osg::NodePathList nodePathList=geode.getParentalNodePaths(stopAt);
osg::NodePath firstNodePath=*(nodePathList.begin());
osg::Matrix matrix=osg::computeLocalToWorld(firstNodePath);
osg::Geode::DrawableList drawableList=geode.getDrawableList();

osg::TriangleFunctor tf;
tf.vertexList=vertexList;
tf.matrix=&matrix;

for(osg::Geode::DrawableList::iterator itr=drawableList.begin();
itr
itr++)
{
(*itr)->accept(tf);
}
traverse(geode);
}
osg::Vec3Array* vertexList;
osg::Group* stopAt;
};

osg::Vec3Array* TriangleConvertor::Convert(osg::Group* group )
{
osg::Vec3Array* vertexList=new osg::Vec3Array;
VertexVisitor vertexVisitor;
vertexVisitor.vertexList=vertexList;
vertexVisitor.stopAt=group;
group->accept(vertexVisitor);
return vertexList;
}
```

思路是使用一个visitor来遍历Group下面所有的Geode,然后用osg::TriangleFunctor获取所有的三角形片.在获取的时候对每一个三角形进行矩阵变换,目的是将local坐标系的三角形数据转换成world坐标系的三角形.获取矩阵的方法是用osg::computeLocalToWorld这个方法.值得注意的是在寻找ParentPath的时候要设置haltTraversalAtNode这个参数为查询起始的Group.这是因为,如果group还有父对象的话,那么会得到所有的对象,而不是返回基于group所在的坐标系的三角形片数据.(可能说的有些绕……)

这样说明吧,举个例子层次结构为一下:

```
Group Root
MatrixTransform Position
MatrixTransform Bike
MatrixTransform FrontWheel
Geode 1
Geode 2
MatrixTransform RearWheel
Geode 3
Geode 4
```

注意一个MatrixTransform也是一个Group.这里我们需要得到Bike整个模型的三角形片,而且整个三角形片受到Position这个矩阵的影响.那么当遍历到Geode 1这里的时候,如果调用geode.getParentalNodePaths(),那么就会返回NodeParentList如下:

```
Root
Position
Bike
FrontWhell
```

这样的话就相当于要把所有的三角形转换成世界坐标系里面的值.不符合我们的要求.那么如果设置了haltTraversalAtNode=Bike,也就是说调用geode.getParentalNodePaths(Bike),那么NodeParentList如下:

```
Bike
FrontWheel
```

这样就满足了我们的要求.然后经过计算的矩阵就是相对于Bike的矩阵,经过转换的三角形片就是整个Bike的三角形片了.
最后整个三角形片通过Vec3Array返回.
