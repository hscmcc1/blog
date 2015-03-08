---
author: hesicong
comments: true
date: 2011-01-26 10:59:24+00:00
layout: post
slug: longer-job
title: 让任务执行得更久一点
wordpress_id: 2920
categories:
- 其他
tags:
- bash
- Linux
- shell
---

花了那么多银子买Dreamhost，总得让这个空间用的更丰富点吧。最近有个想法把“小米分享”网站的所有图片给搞下来，脚本那是相当简单，几句话就可以搞定：(down.sh)

``` bash
    ID=0
    while true
    do
            ID=`expr $ID + 1`
            ID=`expr $ID % 999999`
            wget -crm "http://fx.xiaomi.com/"$ID
    done
```

至于原理就不用说了，很简单。

现在的问题就是如何让这个下载任务能够长期不断的运行下去，之前用后台线程下载，结果ssh一退出就没了。怎么解决呢？

用nohup就可以解决

```
nohup ./down.sh &
```

要看到直接过程，可以用tail查看：

```
tail -f nohup.out
```
