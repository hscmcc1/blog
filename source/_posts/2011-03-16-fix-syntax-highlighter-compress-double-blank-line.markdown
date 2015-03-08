---
author: hesicong
comments: true
date: 2011-03-16 13:08:46+00:00
layout: post
slug: fix-syntax-highlighter-compress-double-blank-line
title: 解决syntax-highlighter-compress双换行符的问题
wordpress_id: 2966
categories:
- 软件
tags:
- highligter
- syntax
- wordpress
---

```
    int test()

    int test1()

    void main()

    {

    }
```

空得很难看。看了一下，原来是TinyMCE按钮的代码的问题：
将：syntax-highlighter-compress/tinymce/window.php中的：

```
if (shc.className.indexOf('current') != -1) {
	var shcla = document.getElementById('shc_lang').value;
	var shcid = document.getElementById('shc_code').value.replace(/</g,'<').replace(/n/g,'<br>');
```

改为：

```
if (shc.className.indexOf('current') != -1) {
	var shcla = document.getElementById('shc_lang').value;
	var shcid = document.getElementById('shc_code').value.replace(/</g,'<').replace(/rn/g,'<br>').replace(/n/g,'<br>');
```

原因是做换行替换的时候，只替换了\n，没有替换\r\n，对于windows版本，先替换\r\n，再替换\n就可以了。