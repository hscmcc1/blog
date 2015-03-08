---
author: hesicong
comments: true
date: 2007-02-03 08:14:34+00:00
layout: post
slug: learning-experience
title: 学习心得
wordpress_id: 126
categories:
- 其他
tags:
- opengl
- OpenSceneGraph
- OSG
---


感冒接近尾声了，真实感觉身体素质那么差喔，一天吃那么多东西都是浪费…………最近又重新开始弄弯管机的玩意儿了，有些学习心得和实际操作心得，留个纪录：
１、关于矩阵乘法
OpenGL规定，最后定义的变换最先应用，每一步操作，都在CMT再右乘一个矩阵。所以坐标架原点p经过放射变换到q，公式如下：

```
q=M1*M2*M3*M4*p
```

其中M1为第一次变换，M2为第二次，以此类推。

特别要搞清楚到底是右乘还是左乘。取决于你是在移动坐标架还是在移动点。

如果是移动点，坐标系不变，则是：

```
q=M4*M3*M2*M1*p
```

相关文章参见《交互式计算机图形学——基于OpenGL的自顶向下方法(第三版)》P136和P145。
２、关于RAPID碰撞检测的调试

这个碰撞检测库需要你提供变换矩阵(tMatrix)和一个包含模型三角形的一个类的实例(model)。model比较好构建，只需要把模型三角形添加进去即可。tMatrix难度较大，需要做矩阵的乘法，往往这个不对会造成碰撞检测的失败。为了真实的看到碰撞检测是否正确，可以：

```
glPushMatrix();
glLoadIdentity();
glTranslate(tMatrix);
model.RenderTriangles();
glPopMatrix();
```

另外可以将碰撞检测所检测到的碰撞三角形显示出来，以重点测试tMatrix变换是否正确。
３、关于程序的一些设计

试验阶段，不必要一定遵循面向对象的一些设计思路，这样反而会弄得很麻烦。前段时间一直想用面向对象的设计思路来封装对象，结果后来一旦发现整体思路不正确，又要改很多很多，真的很麻烦。还不如面向过程，不管是书写还是调试都会有一定的速度优势。而可能更好的一面在于，万一方案不行，改动的时候会方便很多。举个例子：

比如对于一个类的名字：

```
class A{
public:
string GetName();
void SetName(const string* name);
private:
string name;
}
```

然后需要写对应的函数。

还不如直接

```
class A{
public:
string name;
}
```

虽然暴露了成员，但是给调试和写测试程序带来了很多方便。所以，凡是不要绝对化，该用什么的时候还是用什么比较好。
４、被骗

问题来自于3ds文件的Node的名字。他的名字只有10个字符长，但是3dsmax的max中却可以有很长的名字。

所以如果我需要调入GeoSphere01，其实在3ds文件中，只存在名字为GeoSphere0的Node。所以我想直接把Name调整大小成10就是了：

```
string name="GeoSphere01";
string name1="GeoSphere0";
name.resize(10);
if(name.compare(name1)==0)
{
cout<<"Identical"<
}
```

这样正确。

但是遇到这样的情况：

```
string name="Hello";
string name1="Hello";
name.resize(10);
if(name.compare(name1)==0)
{
cout<<"Identical"<
}
```

如果不注意其实用了resize以后，name就变成了Hello\n\n\n\n\n，结果和name1比较，结果不会是相同的(VS2005编译结果)。需要注意！可能是string类的compare函数实现上不以\n作为结尾而是以实际长度作为长度判断标准(瞎猜，要看看相应的文献再说)

５、简单测试内存是否泄漏

简单来说内存泄漏就是new了忘记delete，占用了内存。往往这些在于一个类的实例执行某些函数的时候new了一些东西，而忘记了在析构函数里面删除。

那么如何测试到底有没有泄漏呢？我的简单方法如下：

```
for(int i=0;i<10000;i++)
{
CObj obj1=new CObj();
obj1.doSomeTask();
delete obj1;
}
```

如果忘记了删除的话，你会看到任务管理器里面程序占用了大量的内存。这时候就说明obj1确实存在泄漏问题。如果要确定到底是什么原因造成的，可以在执行这段语句之前纪录下内存使用mem1，然后执行完了看看内存使用mem2。然后(mem2-mem1)/10000就是每一次new的大小，这样顺藤摸瓜总会找到一些没有delete的东西。

当然，更好的办法还是用专用工具。但现在对于我来说还没有用上：)

６、闲话

要早点睡觉，身体那么差了，唉～～～～～～～～～睡觉！
