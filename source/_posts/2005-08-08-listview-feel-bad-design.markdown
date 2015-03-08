---
author: hesicong
comments: true
date: 2005-08-08 09:38:51+00:00
layout: post
slug: listview-feel-bad-design
title: 感觉ListView设计得不好的地方
wordpress_id: 1368
categories:
- 其他
tags:
- .net
---

ListView有Item属性，Item又有SubItem属性。如果

```
Dim newItem As New ListViewItem
newItem.Text = "0"
newItem.SubItems.Add("1")
newItem.SubItems.Add("2")
```

这样访问newItem.SubItems(0).Text会是0

感觉SubItems既然叫SubItem，那么应该第一个，也就是0索引应该是添加到SubItem的第一个的值，这里为1。但是变成了Item.Text的值，有点莫名其妙的感觉……~

我在写程序的时候经常把这个问题搞错(粗心阿)，希望大家在写ListviewItem的时候不要犯这样的毛病了：)
