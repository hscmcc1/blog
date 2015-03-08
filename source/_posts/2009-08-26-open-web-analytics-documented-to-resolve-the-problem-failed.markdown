---
author: hesicong
comments: true
date: 2009-08-26 13:51:29+00:00
layout: post
slug: open-web-analytics-documented-to-resolve-the-problem-failed
title: 解决Open Web Analytics载入文件失败的问题
wordpress_id: 1983
categories:
- 软件
tags:
- .htaccess
- bug
- owa
- wp-content
---

如果你的wordpress安装在子目录，而使用根目录进行访问，会生成一个.htaccess，但这个规则可能会造成子目录访问失败。我这篇文章[cross id=1987]中也提供了一个解决方案，但我发现我的主页中访问子目录还是存在问题。

所以最终我选择了修改Open Web Analytics插件的方式。主要是调整访问wp-content的路径。

查找到wp-plugin.php包含一句：

``` php
    define('OWA_PUBLIC_URL', get_bloginfo('url').'/wp-content/plugins/owa/public/');
```

修改为

``` php
    define('OWA_PUBLIC_URL', get_bloginfo('wpurl').'/wp-content/plugins/owa/public/');
```

即可。此时将使用wordpress绝对路径进行访问。
