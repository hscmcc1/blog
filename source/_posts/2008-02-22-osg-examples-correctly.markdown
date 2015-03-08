---
author: hesicong
comments: true
date: 2008-02-22 03:32:22+00:00
layout: post
slug: osg-examples-correctly
title: 正确运行OSG的例子
wordpress_id: 324
categories:
- 其他
tags:
- opengl
- OSG
---

OSG为我们提供了很多有用的例子，那么如何正确运行这些例子呢？我将我的一些经验写出来，希望对大家有所帮助。

### 命令行参数如何找？###

main函数作为一个程序的入口很多命令行的参数的在这里处理。OSG的例子里很多都是需要提供参数的，否则就需要调用默认的文件。注意如果你直接运行OSG的例子有可能只是一闪而过，正常的，不要惊慌，只需要分析一下到底需要什么命令行参数就可以正确的运行起来。
以osgviewer这个程序来讲，我们看看相关的处理函数：

```
arguments.getApplicationUsage()->setApplicationName(arguments.getApplicationName());  //设置程序的名称
arguments.getApplicationUsage()->setDescription(arguments.getApplicationName()+" is the standard OpenSceneGraph example which loads and visualises 3d models."); //简单的描述
arguments.getApplicationUsage()->setCommandLineUsage(arguments.getApplicationName()+" [options] filename ...");  //例子的使用方法
arguments.getApplicationUsage()->addCommandLineOption("--image <filename>","Load an image and render it on a quad");   //参数
....(省略一些)
```

这里就可以看书这个程序是干什么的，具体的使用方法是什么，以及一些参数的用法。

确定了该传什么参数以后，一来可以在“命令行”里面直接输入指令(如果你比较熟悉的话)另一个方法就是在工程的“属性页“中，选择“调试”，“命令参数”中添加需要的指令。这样就可以让大多数例子运行起来了。

### 确定你的环境变量是正确的 ###

有些时候即便你设置了命令行参数，但是还是出现找不到一些文件的情况。注意OSG的例子需要一些文件来执行，你可以在[这里](http://www.openscenegraph.org/projects/osg/wiki/Downloads/SampleDatasets)找到这些例子需要的文件。

下载好后解压好放置好，例如我这里我放在E:osgOpenSceneGraph-Data，那么我还需要设置环境变量(如何设置请BAIDU)。添加一个名为OSG_FILE_PATH，值为E:osgOpenSceneGraph-Data的环境变量，这样大多数例子不需要你提供额外的文件就可以工作。
另外如果还是报告一些warning，则可能是相应的插件没有找到，请在PATH环境变量中正确设置你的OSG插件的位置。

### 如果还是不工作？###

如果上述都正确了，还是不工作，怎么办呢？确认你的显卡能够正确的支持例子，有些高级的例子需要更新的显卡的支持。例如Examples osggeometryshaders就需要DX10系列的显卡才能正确工作。一般来说如果出错控制台都会输出相应的信息的。有些显卡可能不支持一定的扩展，也是可以根据控制台输出知道的。
确认以上三点以后大部分例子还是能够运行成功的，当然如果你运气实在不好，请跟踪一下源代码，找到病因，那么你会对OSG更加了解的。
