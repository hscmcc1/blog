---
author: hesicong
comments: true
date: 2007-01-23 18:55:34+00:00
layout: post
slug: cool-a-blog
title: 超酷的一个Blog
wordpress_id: 116
categories:
- 其他
---

怎么也没有想到用 console mode 在 command-line 下命令来阅读文章。打开这个blog：http://blog.elinc.ca/rodcli/index.php 。不要以为这是一个Unix/Linux命令行界面，如果你有用过 UNIX /Linux的 console mode，看到这样的界面大概都会下意识地输入 ls 命令(在 UNIX/Linux 中这个命令通常是列出当前目录下的所有文件)，没错！这样就会列出blog的所有的文章 ，还可以用 cls 命令清除屏幕。支持那些命令可以输入help。输入 help 之后列出下列的命令：

WordPress GUI Theme (c) 2006 Rod McFarland

command, alias required [optional]: description

help, h, ?:                    Help (this)
gui, startx:                   Return to GUI (graphical interface) blog
ls, list:                      List all posts (ordered by ID/date)
search, find [search terms]:   Search posts
read, cat, show [post_id]:     Read post # (no post_id: read current)
comments:                      Read comments for current post
current, cursor [post_id]:     Show current post_id (set if post_id
given, nearest higher if no post
matches)
latest, last, l:               Move to and show latest post
next, n:                       Move to and show next post
prev, p:                       Move to and show previous post
first, f:                      Move to and show first post
random, rand, r:               Show random post
comment, c:                    Leave comment on current post
categories:                    List categories
category cat_id | cat_slug:    List posts in category
cloud:                         Display tag cloud
date:                          Current date/time
cls:                           Clear screen
gpl, license:                  Show GNU General Public License
[post_id] | [post_slug]:       Read post
login:                         Login (to admin)

是不是有一种在使用shell的感觉，是不是很酷。这些外国人怎么就这么有创意？
