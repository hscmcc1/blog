---
author: hesicong
comments: true
date: 2007-03-28 23:05:12+00:00
layout: post
slug: openscenegraph-build-guide
title: OpenSceneGraph编译指导
wordpress_id: 167
categories:
- 其他
tags:
- OpenSceneGraph
- OSG
---

OpenSceneGraph是一个很大的开源工程，如果要自己编译的话要花一些功夫，而且开源的经常有一些依赖库的原因经常编译失败。开源还有一个问题就是文档有时候跟不上源代码，这个是最恼火的事情。通过好几天的摸索，反复阅读www.openscenegraph.org上面的相关文章，终于在VC++2005环境下面编译成功，现分享如下：

一、OSG源代码的获取：
* SVN的方式：
参见http://www.openscenegraph.com/index.php?page=Downloads.SVN
缺点：速度比较慢
* NightlyTarballs(推荐，以下叙述都以此为基础)：
可以自动生成最新的源代码。地址：http://openscenegraph.org/downloads/developer
优点：速度快，源代码保持最新。

二、第三方代码获取：

如果你只编译Core osgXXXX的话可以跳过此步骤。
由于OSG的有一些plugin需要第三方库的支持，所以必需要找到相应的库才能编译完成。
从http://www.openscenegraph.com/index.php?page=Downloads.Dependencies可以得到一个WIN32库的压缩包(http://openscenegraph.org/downloads/dependencies/3rdParty_Win32binaries_2005_05_10.zip)

三、准备：

准备一个空目录，这里暂时取名为osg
建立4个目录：
解压下载的NightlyTarballs和3rdParty到这个目录
这时目录结构为：
3rdParty_xxxxxx
OpenSceneGraph_xxxxxx
OpenThreads_xxxxx
Producer_xxxx
重命名这些文件夹为3rdParty, OpenSceneGraph, OpenThreads, Producer

四、编译OpenThreads和Producer

进入OpenSceneGraphOpenThreadswin32_src，打开OpenThreads.dsw，按提示转换项目。
然后各种类型Debug、Release、Debug Static、Release Static各编译一次，得到4种库，方便后面使用。
Producer的编译方法基本相同。

五、编译OpenSceneGraph：

此文为转载，原文请见www.hesicong.net
进入OpenSceneGraphVisualStudio目录可以看到OpenSceneGraph.dsw，直接打开以后VS2005会提示转换为VC8项目，转换并保存sln文件。
由于文件较多，载入时间会相应较慢，请耐心等待。
下面正式开始批量编译。
* 首先保证磁盘有充足的空间。我选择Debug和Release模式编译了Core、Plugin、Example后，OSG目录总共消耗了3.23GB的硬盘空间。如果还有其它需要编译的项目可能需要更大的磁盘空间。
* 菜单－》生成－》批生成
* 选择需要编译的项目。
一般选择配置为Debug和Release的就够了。Debug用于自己写程序调试，Release用于最后的发行。千万不要选择Debug Static和Release Static这种静态链接库，否则消耗大量磁盘空间和编译时间。
编译的项目至少包含Core xxx。osgPlugin和Example视情况而定。
* 按“生成”就开始了漫长的编译过程，这时候可以出去晒晒太阳，或者出去好好的每餐一顿。如果还编译了Example的话那么推荐再睡一觉，这样就有更多的精力处理编译出错的问题：)

六、一般编译错误问题：
此文为转载，原文请见www.hesicong.net
* 找不到一些库，常见于OpenThread32.lib，Producer.lib等。这时候需要确认所有目录是不是放置正确。确认生成了必要的文件。
* 找不到头文件。如果是编译osgPlugin出错的话，你可以试着寻找一下所需要的其它库。3rdParty里面的库不完整，比如新的osgPlugin COLLDIA dae就无法编译，少了几个头文件。
* 找不到导出的函数。例如说找不到osgSim::xxxxxxx，确认你生成了osgSim这个项目。
* 更多问题：订阅OpenSceneGraph Mailing-List

七、运行Examples
编译好的examples最好在shell里面运行。
进入osgOpenSceneGraphVisualStudio，运行osgShell.bat。这个批处理文件会注册一些变量以便程序使用。
此文为转载，原文请见www.hesicong.net

八、如果还有问题
确认按照此步骤进行。如果还有问题，请留言。
其它问题请订阅OpenSceneGraph Mailling-list，前提是需要有一个良好的英文基础：)
祝大家成功！为转载，原文请见www.hesicong.net
