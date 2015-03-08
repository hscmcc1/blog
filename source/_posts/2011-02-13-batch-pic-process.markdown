---
author: hesicong
comments: true
date: 2011-02-13 14:15:29+00:00
layout: post
slug: batch-pic-process
title: 大量照片批量处理
wordpress_id: 2930
categories:
- 软件
tags:
- bash
- imagemagick
- pic
- resize
- 批量
- 照片
---

自从有了数码设备以后，照片便越来越多了，并且随着从600W像素升级到1400W像素，硬盘空间及备份都成了问题。几年的照片已经有1万多张，加上一些视频已经占用了50G的空间。想备份一下，免得哪天硬盘挂了就傻逼了。如果直接拷贝的话，倒是没问题，关键是要找一个硬盘来对付这么大的存储。其实我是想备份到网络上的，DreamHost提供了50G的免费存储空间，为何不用呢？再加上有时候需要将照片共享一下，照片缩放自然成了一个大问题。

当然，我不可能将50G都传上去，显然俺们的小管子是不适合做这个事情的。那么只有先压缩了。我试过用photoshop写了个action跑了一晚上，发现中途出错了，可控性也不好；用一些宣称为批量处理的软件，结果一个比一个傻逼；有些软件竟然最后自己崩溃了，简单看了一下原来是自己将所有的图片都读了一遍，把资源消耗完了。

总结起来现有软件有以下不足：

1. 无法对付海量图片
2. 性能不好，速度太慢
3. 不支持多线程，浪费能源
4. 对文件的可操控性差，操作缓慢

最后实在是没办法了，操起cygwin，写个脚本搞吧。我一直知道，在linux下面工作，用脚本，那就是另一种异常开心的工作。

首先试了试ImageMagick的缩放素质，还是蛮不错的，速度也挺快。好，下一步就是写脚本，在try and retry过程，终于出了个可用的脚本：

该脚本可以批量进行处理，并且是多线程的，很黄很暴力。在我的Q6600上实测，开16个进程，CPU占用率一直100%，内存在1G-1.5G之间波动，10多秒就转换完16个文件，速度超快~哈哈。

``` bash
    #!/bin/bash
    #将分隔符设置为n，避免路径中有空格造成分割失败
    export IFS=$'n'

    #源路径
    srcDir=$1

    #目的路径
    dstDir=$2

    cd $srcDir
    mkdir $dstDir
    echo $dstDir

    #从源路径搜索所有为jpg格式的文件
    rst=`find . -iname '*.jpg'`

    #放到数组里面
    declare -a files

    i=0

    for aFile in $rst
    do
            files[$i]=$aFile
            i=$i+1
    done


    fileCount=${#files[@]}
    #cores个进程一起处理
    cores=16

    for (( i=0; i<$fileCount;i+=$cores ))
    do
        for (( j=0;j<$cores;j++ ))
        do
          {
            id=$(expr $i + $j) #任务ID

            file=${files[$id]}  #文件名
            dst=$(dirname $file) #得到目的目录

            absDstPath=$dstDir${dst:2}  #得到目的文件夹
            mkdir -p $absDstPath  #建立目的文件夹
            filename=$(basename $file)    #得到文件名
            absDstFilePath=$absDstPath/$filename  #得到目的文件绝对路径

            if [ -e $absDstFilePath ]
            then
                  #文件存在，直接跳过
                  echo "[$id/$fileCount] skip "$absDstFilePath
            else
                  #调用convert进行处理，这里将图片缩放50%
                  echo "[$id/$fileCount] converting "$absDstFilePath
                  convert -resize %50x%50 "$file" "$absDstFilePath"
            fi
          } &
        done

        #等待所有进程完成
        wait
    done
```