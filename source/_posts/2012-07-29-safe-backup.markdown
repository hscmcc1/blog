---
author: hesicong
comments: true
date: 2012-07-29 04:57:51+00:00
layout: post
slug: safe_backup
title: 金山快盘 + Super Flexible File Synchronizer = 安全备份方案
wordpress_id: 3109
categories:
- 生活
---

硬盘里面几十个G的东西，找不到地方存储。放在硬盘里面终究不是个好事。DreamHost虽然提供了50G的免费存储空间，但是国外的服务器上传速度是个问题，并且浏览起来还是不怎么方便。

金山快盘还是比较好用，上传速度比较快，但唯一让我担心的是安全问题。我只能假设所有的网盘都存在技术问题，存在可能泄露用户资料的可能。那唯一我们能做的，就是上传之前，将文件加密了，然后用的时候再解密。能做到这点的软件不多，以前在挑选备份软件的时候，发现Super Flexible File Synchronizer提供了一个加密备份选项

[![image](/images/2012/07/image_thumb.png)](/images/2012/07/image.png)

[![image](/images/2012/07/image_thumb2.png)](/images/2012/07/image1.png)

[![image](/images/2012/07/image_thumb3.png)](/images/2012/07/image2.png)

通过打开这个选项，我们可以用ZIP加密的方式，将文件同步到快盘的文件夹。这样，在网盘上的文件就是加密了的。

[![image](/images/2012/07/image_thumb4.png)](/images/2012/07/image3.png)

每个文件都是以sxxxxxx.zip作为后缀。

后续，需要解密的时候，仅需要将文件重新同步一下就可以了。
