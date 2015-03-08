---
author: hesicong
comments: true
date: 2009-12-26 04:19:47+00:00
layout: post
slug: solution-under-svn-commit-error-in-windows-7-issues
title: 解决SVN在Windows 7下commit出错的问题
wordpress_id: 2127
categories:
- 软件
tags:
- commit
- error
- svn
- win7
---

自从操作系统升级到Win 7以后，SVN Commit就经常出现类似错误：

> Commit
> G:doc\projects\estar\trunk\vs\EstarSharp\EstarSharp\DataWorkData.cs
> G:doc\projects\estar\trunk\vs\EstarSharp\EstarSharp\GUIFormsAbout.cs
> G:doc\projects\estar\trunk\vs\EstarSharp\EstarSharp\GUIFormsMainForm.cs
> G:doc\projects\estar\trunk\vs\EstarSharp\EstarSharp\GUIFormsMainForm.cs
> G:doc\projects\estar\trunk\vs\EstarSharp\EstarSharp\DataWorkData.cs
> G:doc\projects\estar\trunk\vs\EstarSharp\EstarSharp\GUIFormsAbout.cs
> Commit succeeded, but other errors follow:
> Error bumping revisions post-commit (details follow):
> Can't move
> 'G:doc\projects\estar\trunk\vs\EstarSharp\EstarSharp\Data.svntmpentries' to
> 'G:doc\projects\estar\trunk\vs\EstarSharp\EstarSharp\Data.svnentries': The
> file or directory is corrupted and unreadable.

说文件损坏或者无法读取。这是一个很令人费解的问题，到底commit是成功还是失败了？网上查询一番后找到有朋友遇到相同的问题并且给出了解决方案(见：http://schleichermann.wordpress.com/2009/12/09/svn-tortoisesvn-cant-move-the-file-or-directory-is-corrupted-and-unreadable-windows-7/)

原因是Win7启动了索引服务和SVN Commit时候移动文件冲突了。解决方法是关闭Win 7的对SVN仓库的索引服务。

打开Win7的控制面选，选择“索引选项”，然后选择“修改”，将`G:\doc\projects\estar`全部反选即可
