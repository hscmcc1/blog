---
author: hesicong
comments: true
date: 2009-08-26 08:16:06+00:00
layout: post
slug: url-rewrite-to-solve-installation-visit-to-install-wordpress-in-a-subdirectory-of-the-root-problem-occurs-404
title: URL Rewrite解决安装访问安装在根目录的Wordpress的子目录出现404的问题
wordpress_id: 1978
categories:
- 软件
tags:
- '404'
- owa
- rewriterule
- urlrewrite
- wordpress
---

我真不知道这个题目应该怎么写了，呵呵。

问题是这样的，今天装了Open Web Analytics(OWA)后，出现OWA无法用的问题。用Firebug查看网络请求发现OWA请求位于/wp-content/plugins/owa目录中，而直接访问这个目录会出现404错位，因为我的wordpress是装在/b/目录下的，由.htaccess来实现wordpress在根目录中访问。wordpress默认的url-rewrite是这样：

```
 # BEGIN WordPress

 RewriteEngine On
 RewriteBase /
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule . /index.php [L]
```

根据Apache的文章可知，RewriteRule的意思是把所有的请求都给index.php处理。

那么要解决我的问题，我应该添加一条规则

```
RewriteRule ^wp-content/(.*)$ /b/wp-content/$1 [R]
```

这样完整的规则是：

```
# BEGIN WordPress
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^wp-content/(.*)$ /b/wp-content/$1 [R]
RewriteRule . /index.php [L]
# END WordPress
```

可是这样依然无法访问。原因是Apache会遍历每一个规则。查看了[《Apache中 RewriteRule 规则参数介绍》](http://www.52web.com/52article/?view-80.html)一文，重点是这个： 'last|L'(结尾规则) 立即停止重写操作，并不再应用其他重写规则。它对应于Perl中的last命令或C语言中的break命令。 这个标记用于阻止当前已被重写的URL被后继规则再次重写。 例如，使用它可以重写根路径的URL('/')为实际存在的URL(比如：'/e/www/')。 所以，最终的.htaccess应该这样写：

```
# BEGIN WordPress
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^wp-content/(.*)$ /b/wp-content/$1 [R,L]
RewriteRule . /index.php [L]
# END WordPress
```

以后再出现这种在wordpress安装目录下子目录无法访问的情况就类似的这样解决。
