---
author: hesicong
comments: true
date: 2007-07-23 02:57:52+00:00
layout: post
slug: blog-updated
title: Blog Updated
wordpress_id: 228
categories:
- 其他
---

最近发现BLOG的文章看起来比较吃力，发现是没有首行缩进和段落空行的原因。然后就动手找解决方法。在看了pjblog的源代码和网上的一些零散的说法以后，找到了显示文章对应的文件classcls_article.asp。这个文件又有几个包含文件，查看了许久没有找到真谛。ASP的页面也真是麻烦，只有找一些地方输出一些值来进行调试了。后来尝试了许久，都已失败告终。后来想到更改数据库，因为pjblog同时支持UBB和FCK两种编辑方式，所以简单的想吧UBB里面
的部分改成

就对了，结果大失所望，日志输出全部乱套。
吃饭之前发现ubbcode.asp里面有窍门，决定对它动手。多次尝试以后，在函数返回之前加上以下几句：

```
'段落 		//By hesicong
strContent=Replace(strContent,"
","

")
UBBCode=strContent
```

UBB自带的换行转换成\n的问题解决，现在页面都是

了，这样就好做首行缩进了。
然后就是将主题对应的layout.css文件更改，加上“text-indent:2em;”就对了。最终感觉基本像话，当然粗暴的替换
标示可能带来一些问题，而且现在写文章的时候也不能随便换行了，要不然会更难以阅读。
