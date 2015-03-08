---
author: hesicong
comments: true
date: 2007-03-07 06:15:49+00:00
layout: post
slug: find-a-specific-node-osg-method
title: OSG中找到特定节点的方法
wordpress_id: 150
categories:
- 其他
tags:
- OpenSceneGraph
- OSG
---

为了在OSG中找到需要的节点并对节点做出相应的操作，可以从NodeVisitor类中继承一个类，NPS的教程![下载文件](images/download.gif)

[download id="14"]
阐述了这个问题。下面是我写的一个类，找到指定名字、指定类型的节点：

```
class findGeoNamedNode:
public osg::NodeVisitor
{
public:
findGeoNamedNode();
findGeoNamedNode(const std::string name):
osg::NodeVisitor(TRAVERSE_ALL_CHILDREN)  //Set traverse mode
{
resultNode=NULL;
this->name=name;
}

virtual void apply(osg::Node &searchNode)
{
if(searchNode.getName()==name)
{
osg::Geode* dynamicTry=dynamic_cast(&searchNode);

if(dynamicTry)
{
resultNode=dynamicTry;
}
}
traverse(searchNode);
}

osg::Geode* getNode()
{
return resultNode;
}
private:
osg::Geode* resultNode;
std::string name;
};
```

使用这个VISITOR类只需要调用以下的一些函数

```
osg::Node* testNode=NULL;
testNode=dynamic_cast(osgDB::readNodeFile("d:\1.3ds"));

findGeoNamedNode* visitor=new findGeoNamedNode("Box01");
testNode->accept(*visitor);
```
用起来很方便，得益于visitor模式的正确应用。
