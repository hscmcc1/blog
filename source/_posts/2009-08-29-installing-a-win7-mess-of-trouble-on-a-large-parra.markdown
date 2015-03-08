---
author: hesicong
comments: true
date: 2009-08-29 11:13:26+00:00
layout: post
slug: installing-a-win7-mess-of-trouble-on-a-large-parra
title: 装个WIN7惹了一大帕拉麻烦事情
wordpress_id: 1987
categories:
- 硬件
- 软件
tags:
- acronis
- ghost
- mbr
- pq
- win7
- 分区表
- 硬盘灯常亮
---

最近C盘空间已经只有最后10%了，我的习惯是50G的C盘空间只要用得还有5G了，就可以重装了。好吧，也是时候了，前几天下载了WIN7还正愁没有时间装呢。

刻盘，浪费一张DVD；刷BIOS，刷成DELL OEM的，破解正版；重启；进入安装界面。

噩梦来了。

在磁盘分区的时候，由于WIN7需要一个额外的分区，而现在的盘没有更多的空间了，所以简单的可以把C盘删除了再新建就好。我以前用XP的时候就知道微软的分区那是一个造孽，一旦以你的分区稍微有点不标准就会把磁盘搞得一塌糊涂。比如我以前装linux的时候，将一个分区搞成了主分区，成了这样：主分区，扩展分区，主分区，扩展分区。然后在XP里面删除一个盘，这下，所有的扩展分区都没了。分区表全部坏了。微软的分区工具就是这么破，要多个心眼。

好了，删除了分区，这下鼠标就一直是圈圈转啊转，转啊转，硬盘灯常亮。没反应了。

重启，使用了Acronis Disk Suite也无法进入到分区，停到了Analyse Partition C这一个阶段。以前的工具对SATA支持不好，就像“经典”的Partition Magic，都不太好使。反正这样那样的工具都不行，好吧，那就用Partition Magic试试。嘿，还好，找到硬盘了，给我说有一个分区错误，提示说要重启看结果。我心想这下好了，修正吧。重启，傻眼了。

Win7系统安装进不了；光盘启动的XPE系统进入不了，硬盘灯常亮；Linux系统无法mount磁盘分区；几种DOS系统均无法看到Starting DOS，直接死到。好吧，这下没系统可以用了，PQ也进不了，几乎所有的工具都被无敌了。拆下来放到老爸的机器上看看行不行。

在XP里面直接挂上硬盘(SATA可以热拨插的)，提示驱动程序无法正确加载。我郁闷了。好吧，重启，照样不能启动。试了很多次，也是如此。放弃了。

我都想哭了，装了WIN7就这么恼火？把以前的老工具都找出来用用，最后找到了个Ultimate Boot CD中的DOS工具集竟然能启动，启动后看到个paraman.exe竟然能够识别出分区信息。

根据以往的经验，应该是MBR坏了，造成磁盘无法读取。将MBR全部写零并将C盘的头几百个KB写零。这样造成的结果就是所有的分区都丢失了，不要着急，并不是意味着数据也丢失了。

插入Win7光盘，终于见到了分区界面。显示是一个新硬盘。正常的，因为MBR已经全部损坏了。我的C盘是50G，所以暂时分45G，以免破坏以后分区的数据，装完了再还无损调整分区就可以了。安装完Win7后，第一件事情就是下载亲爱的Acronis Disk Suite。忘了你的什么PQ和GHOST吧，这两个软件已经廉颇老矣，用Acronis出品的Disk Suite和True Image才是王道。

下载的时候可别网上一搜就下载，多半是DEMO版本的，告诉你，到这里来下载：http://58.251.57.206/down?cid=7DA00927EDE61CBDADC9C3DDFFCC304D61E88642&t=2&fmt=&usrinput=acronis%20disk%20director%20suite&dt=2006000&ps=0_0&rt=0kbs&plt=0

可以直接在“迅雷搜索”里面输入“acronis disk director suite”，下载“支持vista的分区工具Acronis Disk Director Suite 独家提供无错汉化版下载Acronis.Disk.Director.Suite.v10.0.2160.by.ANimal!”就可以了。

按照默认的安装。完毕后重启。

使用Acronis Recovery Expert对剩余的空间进行恢复。一般选择Manual和Fast就可以了，因为所有的其他分区都还在，并且NTFS的MFT还是存在的，所以很容易就搜索到了。FAT32格式的我没有试过，估计也可以，只是NTFS在Windows系统中，算是最安全的文件系统了。

不久就会问你找到的分区是不是，看看卷标、大小和剩余空间就可以了。整个过程用不了几分钟就完成。这样数据就完璧归赵了。

**总结一下：**

. 硬盘硬件能够识别但软件不能识别或硬盘灯常亮，经常是MBR出了问题，解决方法是试图修复或者清除MBR。随后用Acronis Recovery Expert来恢复就可以。不推荐Easy Recovery，只能恢复文件而且多半用不了，并且要占用另外的空间。

. 放弃使用PQ和GHOST之类的老软件。用Acronis一套吧，听说paragon partition manager也很不错，不知道我运行的paraman.exe是不是这个dos工具。

. 多准备点工具，以备不时之需。(刚才那个工具似乎是：http://www.ultimatebootcd.com/download.html 里面的)
