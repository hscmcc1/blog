---
author: hesicong
comments: true
date: 2014-02-16 12:13:24+00:00
layout: post
slug: chrome-remote-debugging-blank-webview-inspect-window
title: Chrome远程WebView inspect显示空白
wordpress_id: 3289
categories:
- 其他
---

前一篇文章提到了Chrome提供了远程inspect WebView的功能，给Cordova的程序开发提供了很多的便利。

可是用了没多久就遇到了一现象，点了inspect以后弹出一个空窗口，无法调试。试过重启手机、电脑、重装Chrome，似乎都不管用，偶尔能够用一会儿。

后来用了GoAgent发现了一个秘密，工具会访问chrome-devtools-frontend.appspot.com:443这个地址，但由于伟大的长城的存在，这个地址有时候会无法访问，所以会弹出一个空窗口了。

解决方法就是用代理访问，就可以了。
