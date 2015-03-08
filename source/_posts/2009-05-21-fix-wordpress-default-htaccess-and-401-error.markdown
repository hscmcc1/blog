---
author: hesicong
comments: true
date: 2009-05-21 14:56:58+00:00
layout: post
slug: fix-wordpress-default-htaccess-and-401-error
title: Fix Wordpress default .htaccess and 401 error
wordpress_id: 1780
categories:
- 软件
tags:
- .htaccess
- '401'
- wordpress
---

I encourted a problem that I couldn't see the dialog for my password protected pages. The problem was caused by Wordpress default .htaccess file which redirect 401 page to 404 page. The more detail can be found [here](http://www.andrewrollins.com/2008/01/22/wordpress-and-htaccess-password-protected-directories/) (http://www.andrewrollins.com/2008/01/22/wordpress-and-htaccess-password-protected-directories/)

I've tried three methods the article said and only found the last one is OK. I created a 401 page and put this line into .htaccess and every went well.

```
ErrorDocument 401 /401.shtml
```
I didn't know the reason for the failure of another two failed because my new hosting server is LiteSpee which may not fully compatible with Apache.
