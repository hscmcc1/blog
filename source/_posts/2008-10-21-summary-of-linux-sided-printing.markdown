---
author: hesicong
comments: true
date: 2008-10-21 04:29:35+00:00
layout: post
slug: summary-of-linux-sided-printing
title: Linux双面打印概要
wordpress_id: 387
categories:
- 软件
tags:
- Linux
- print
---

> 摘自：http://www.realss.cn/kb&menuaction=phpbrain.uikb.view_article&art_id=6
**双面打印及小册子打印**

网络打印机常常不能像本机上安装的打印机一样实现一步一步指导地双面打印。这样就需要操作人员格外用心，自己在打印时候先打印一面，然后去打印机前重新装纸，回到桌前再打印另一面。

打印机有两种送纸方式。一般激光打印机送纸时，第一张送入的纸，将会出现在第一页(先进先出)；喷墨打印机第一张送入的纸，将会出现在最后一页。这两种打印机双面打印的方法和小册子打印方法都不同。

对于激光打印机，双面打印需要先逆序打印左页，回到打印机前装纸，再正序打印右页。打印小册子时候，需要先正序打印右页，回到打印机前装纸，再逆序打印左页。

对于喷墨打印机，双面打印需要先正序打印奇数页，回到打印机前装纸，再正序打印偶数页。打印小册子时候，需要先
1. 逆序打印左页，回到打印机前装纸，再逆序打印右页。这样打印出的小册子是先出版芯(不是先出封面)，当左页打印完的时候，需要反过来打右页，并适情况添加一张封页(用于打印封一和封四)。
2. 正序打印右页，回到打印机前装纸，再正序打印左页。这样打印出的小册子是先出封面，适合封面使用特殊纸张的情况。

**从ps或pdf文件打印小册子**

以激光打印为例，分三步：

```
zhangweiwu@joe:~> psbook herzog.ps  booklet.ps
zhangweiwu@joe:~> lpr -o number-up=2 -o page-set=odd booklet.ps
zhangweiwu@joe:~> lpr -o number-up=2 -o page-set=even -o outputorder=reverse booklet.ps
```

**打印小册子时的检查表**

小册子打印最容易出问题，皆因为纸张奇偶数、方向、正反面、打印质量及顺序无一不需要操心，加之打印时间长，印反面时候又容易再错。即使面面俱到，最后仍然挂一漏万。所以列出这个检查表帮助减少失误。
1. 检查文件版本是否正确。小册子一般是有多个版本的，确定一定是在使用最新版。
2. 检查变量设置是否正确。如EIP手册“受众”可能是管理员，而您正在准备打印用户手册。
3. 打印之前，检查目录、域代码、索引是否全部正确，是否已经设置为不显示隐藏段落和隐藏字符(否则页码可能是错的)用OpenOffice  “工具” 里的“全部更新”更新文档。
4. 保存文档以免印反面之前误关闭程序。
5. 打印时要检查设置：纸张尺寸是否正确，纸张朝向要确认是Landscape(横向)，打印精度是否正确；
