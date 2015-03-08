---
author: hesicong
comments: true
date: 2011-03-15 13:51:33+00:00
layout: post
slug: shell-file-backup
title: 几句话解决文件备份问题
wordpress_id: 2952
categories:
- 软件
tags:
- bash
- cygwin
- shell
- 备份
---

电脑上的文档已经有50多个G了，里面杂七杂八的放了一些东西，还有很多照片视频等等。这些照片确实太占地方了，需要缩小一点存储起来，备份嘛，不需要那么高的清晰度，以前的50%就可以了。需求很简单：

* 对JPG文件进行压缩
* 其他文件有更新的再拷贝，无更新拷贝之前是用shell写了一个脚本，觉得太长了。最近也在学shell脚本，用管道可以解决：

``` bash
dstDir=/cygdrive/g/test/
srcDir=/cygdrive/h/doc

cd $srcDir
find |
    sed -r "s:^(.*(jpg|JPG))$:cp -v --parents "1" "$dstDir" && 
    	convert -resize 50%x50% "$dstDir1" "$dstDir1":g" |
    	sed -r "s:^(..*)$:cp -vu --parents "1" "$dstDir":g" |
    	sh --verbose
```

脚本原理简单，用sed替换一下就OK。
