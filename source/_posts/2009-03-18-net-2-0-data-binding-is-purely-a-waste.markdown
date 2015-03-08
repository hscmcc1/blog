---
author: hesicong
comments: true
date: 2009-03-18 05:23:52+00:00
layout: post
slug: net-2-0-data-binding-is-purely-a-waste
title: .NET 2.0数据绑定纯粹是垃圾
wordpress_id: 1651
categories:
- 其他
tags:
- .net
- databinding
- 数据绑定
---

我在开发WinForm应用程序的时候很少使用数据绑定，以前也没有怎么用过。今天在做一个参数设置面板的时候用到了他。然而很令我失望，它不仅没有解决问题，而且还带来了严重的问题，让我找了好半天，最后确认是BUG。当然也不是我一个人遇到了，看看[这篇文章](http://weblogs.asp.net/psteele/archive/2006/10/10/Data-Binding-fails-me-again_2E002E002E00_.aspx)，他们也遇到了相同问题。

问题描述起来是这样的：

我有一个这么样一个Class：

``` csharp
public class BindTest
{
private int number = 5;

private int anotherNumber = 10;

public int Number
{
get { return number; }
set { number = value; }
}

public int AnotherNumber
{
get { return anotherNumber; }
set { anotherNumber = value; }
}
}
```

窗体上有两个控件，一个是textBox1，绑定Number属性，另一个是textBox2，绑定AnotherNumber属性。由于我需要“确认”和“取消”按钮，所以我在高级绑定里面将两个控件的绑定行为改为Never，也就是不更新它的值。

按照MSDN里面阐述的，使用Binding的WriteValue()方法就可以将数据更新。在我的程序里面可以这样写：textBox1.DataBinding[0].WriteValue();

textBox2类似的做法。

后来一运行，问题就出来了，仅仅能够将textBox1的值更新，而textBox2的值还原了！！！

一番搜索以后又回到了刚开始的那篇讨论中，有人这么说道：

As to the problem above this comment, it is a common malfunction of databinding. Simply, if you attempt to control the writing/reading behavior of Binding class, it will --REVERT-- changes of all other non-bound fields/properties. Therefore, only first bound control gets updated due to the fact it is the only one which has access to CHANGED data, after its done with the update, it reverts the changes back thus canceling them, so other bindings are useless.

大意是“这是数据绑定的一个故障。简而言之，试图操作绑定类的写入和读取行为，它回将所有的未绑定的字段和属性还原。因此，仅仅只有第一个被绑定的对象能够得到修改了的数据，在它完成刷新操作之后，它将所有的修改还原因此就取消了其他控件的修改，因此绑定是无用的”

理解了以后，就明白了，我得重新手动写了，哎，数据绑定真是讨厌啊。
