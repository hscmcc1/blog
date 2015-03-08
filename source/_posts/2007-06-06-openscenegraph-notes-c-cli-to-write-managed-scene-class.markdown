---
author: hesicong
comments: true
date: 2007-06-06 04:33:12+00:00
layout: post
slug: openscenegraph-notes-c-cli-to-write-managed-scene-class
title: OpenSceneGraph 笔记--C++/CLI写托管Scene类
wordpress_id: 204
categories:
- 其他
tags:
- c
- C++/CLI
- OpenSceneGraph
---


最近学了C++/CLI，也写了一些小玩意儿体验了它的强大，昨天开始筹划将以前的弯管机的模拟程序用C++/CLI重写。

基本思路是将底层3D部分和上层GUI图形界面部分大体分离。最原始的做法是写一个C++的类，然后定义一些接口，然后用C++/CLI写一个Wrapper，然后用C#进行调用。这种做法其实不是很好，增加了很大的工作量，而且在写Wrapper的时候难免有很多重复性的赋值代码。

第二种思路就是直接用C++/CLI开始写，将Native部分和Managed部分合并在一块儿写。当然C++/CLI有一些限制，不能在托管类里面直接嵌套非托管类，只能有非托管类的指针等等。这个限制带来的最大的不好是osg::ref_ptr，也就是OpenSceneGraph里面的智能指针无法使用了，因为他是一个类型，不能直接嵌入到托管类里面，所以类似下面的语法是错误的：

```
ref class ManagedClass
{
    osg::ref_ptr node;
}
```
当然这样写是正确的：

```
ref class ManagedClass
{
    osg::Node* node;
}
```
但这样就失去了智能指针的保护，很容易造成内存泄露，所以当务之急是需要写一个智能指针来代替osg::ref_ptr，但基本上要保持功能的不变。OpenSceneGraph的引用类都是继承与osg::Object，而osg::Object又是继承于osg::Reference。所以这些引用类都有ref()和unref()方法，用于增加和减少ReferenceCount，当ReferenceCount=0时，就自动delete了。

参考osg::ref_ptr并去掉这个类中不常用的部分，写了一个smart_ptr类，完成了智能指针的任务：

```
    //! OpenSceneGraph managed smart_ptr.
    template
    public ref class smart_ptr
    {
    public:
    typedef T element_type;

    smart_ptr() : _ptr(0) {}
    smart_ptr(T* ptr) : _ptr(ptr) { if (_ptr) _ptr->ref(); }
    smart_ptr(const smart_ptr% rp) : _ptr(rp._ptr) { if (_ptr) _ptr->ref(); }

    ~smart_ptr() { if (_ptr) _ptr->unref();  _ptr = 0; }

    smart_ptr% operator = (const smart_ptr% rp)
    {
    if (_ptr==rp._ptr) return *this;
    T* tmp_ptr = _ptr;
    _ptr = rp._ptr;
    if (_ptr) _ptr->ref();
    // unref second to prevent any deletion of any object which might
    // be referenced by the other object. i.e rp is child of the
    // original _ptr.
    if (tmp_ptr) tmp_ptr->unref();
    return *this;
    }

    inline smart_ptr% operator = (T* ptr)
    {
    if (_ptr==ptr) return *this;
    T* tmp_ptr = _ptr;
    _ptr = ptr;
    if (_ptr) _ptr->ref();
    // unref second to prevent any deletion of any object which might
    // be referenced by the other object. i.e rp is child of the
    // original _ptr.
    if (tmp_ptr) tmp_ptr->unref();
    return *this;
    }

    //T% operator*()  { return *_ptr; }
    T* operator->()  { return _ptr; }
    T* get()  { return _ptr; }

    bool operator!()    { return _ptr==0; } // not required
    bool valid()        { return _ptr!=0; }

    T* release() { T* tmp=_ptr; if (_ptr) _ptr->unref_nodelete(); _ptr=0; return tmp; }

    private:
    T* _ptr;
    };
```
如此这般折腾以后，终于可以在托管类中间使用智能指针了：

```
    public ref class Scene
    {
    protected:

    smart_ptr gc;
    smart_ptr root;
    smart_ptr viewer;
    smart_ptr camera;
    ....
```
跨越了智能指针的障碍以后，还有很多问题有待于解决。像osg::Vec3这些常用类只能重写以便于调用。像查找节点FindNode这种函数：

``` C++
    ref class NodeFound
    {
    public:
    String^ name;
    smart_ptr osgNode;
    };

    NodeFound^ FindNode(String^ name)
    {
    FindNodeVisitor findNodeVisitor;
    findNodeVisitor.name=MarshalString(name);
    root->accept(findNodeVisitor);
    if(findNodeVisitor.node==NULL) throw gcnew Exceptions::NodeNotFoundExpection();

    NodeFound^ nodeFound=gcnew NodeFound();
    nodeFound->name=name;
    nodeFound->osgNode=findNodeVisitor.node;
    return nodeFound;

    return nullptr;
    }
```
只能定义一个新的结构作为返回值，否则C#语言无法使用，因为它不能解析一个智能指针。……或许，还有别的方法可以用吧，比如用IntPtr这种，但可能又会脱离了智能指针的保护，变的危险起来。
