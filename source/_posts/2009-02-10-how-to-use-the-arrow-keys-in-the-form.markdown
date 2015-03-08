---
author: hesicong
comments: true
date: 2009-02-10 11:03:27+00:00
layout: post
slug: how-to-use-the-arrow-keys-in-the-form
title: 如何在Form中使用方向键
wordpress_id: 1589
categories:
- 其他
tags:
- arrow key
- c
- windows forms
---

其实很简单，之前我还用钩子函数来解决，其实根本没必要。重写Form的方法就可以了。

``` csharp
const int WM_SYSKEYDOWN=260;
const int WM_KEYDOWN=256;

protected override bool ProcessCmdKey(ref Message msg,Keys keyData)
{
if ((msg.Msg==WM_KEYDOWN)||(msg.Msg==WM_SYSKEYDOWN))
{
switch (keyData)
{
case Keys.Up:
MessageBox.Show("up");
break;
case Keys.Down:
MessageBox.Show("down");
break;
case Keys.Left:
MessageBox.Show("left");
break;
case Keys.Right:
MessageBox.Show("right");
break;
}
}
return true;
}
```

补充：经实验此法仅能提取到KEYDOWN信息，而KEYUP信息被“吃”了，所以有些需要KEYUP事件的东西就不能做了。

最终在CodeProject找到一个类来处理这个事情，很方便：http://www.codeproject.com/KB/cs/globalhook.aspx
