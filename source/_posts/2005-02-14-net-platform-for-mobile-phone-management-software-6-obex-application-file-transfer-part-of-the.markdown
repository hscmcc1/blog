---
author: hesicong
comments: true
date: 2005-02-14 15:35:30+00:00
layout: post
slug: net-platform-for-mobile-phone-management-software-6-obex-application-file-transfer-part-of-the
title: .net平台手机管理软件开发(6)OBEX应用——文件传输部分
wordpress_id: 1197
categories:
- 其他
tags:
- obex
---

(六) OBEX应用——文件传输部分

在手机数据传输方面基本OBEX应用分为
l 文件传输
l IrMC同步

文件传输又可以细分为以下基本操作
l 初始化连接
l 断开连接
l 设置路径
l 取得目录信息
l 创建目录
l 上传下载文件
l 删除文件或空目录

在笔者的软件当中设计了OBEX这个类，里面包含了以上所有的基本操作。另外针对M55的服务端的特殊性又设计了更名、取得磁盘空间信息、移动、拷贝文件的功能。具体请参考源代码。

下面具体讲述各个操作的细节。
l 初始化连接

初始化连接包括了使手机进入OBEX状态再到发送Connect指令的一系列过程。具体流程参考下图。
ATÆAT^SQWE=0ÆAT^SQWE=3ÆConnectÆ连接到Folder-Listing Service

其中AT^SQWE=0和AT^SQWE=3是西门子特有的隐藏的AT指令，甚至在官方的AT指令集里面都没有提到。其作用是初始化手机到OBEX模式。

发送Connect指令收到手机回复以后确定Max Packet Length等参数。最后连接到Folder-Listing Service进行文件操作。如果需要IrMC同步的话，在Connect后直接连接到IrMC Sync Service即可，手机立刻进入同步模式，所有的应用程序退出。

在笔者的程序中当中首先使用AT指令确定手机当前的工作，如果超时，尝试发送+++并等待1秒钟以便手机从不正常的OBEX状态中退出。然后在此发送AT，成功后即进行文件操作，否则引发一个错误。
l 断开连接

这里的断开连接是指从OBEX模式退出到AT状态中。在AT指令中，连续发送三个0x2B然后等待一秒钟以上即可退出数据模式进入常规AT模式。
l 设置路径

程序中使用SetPath操作设置路径。需要注意的是，可以设计两种风格的过程：一种使用绝对路径，另一种使用相对路径。

对于手机而言，经笔者实践证明，使用绝对路径要比使用相对路径方便，而且更准确，但效率上稍逊，特别是在多层目录的情况下。由于手机没有能够返回当前路径的方法，所以相对路径总难以控制，只有通过程序控制，极容易出现错误。所以建议使用绝对路径。

如果使用绝对路径，那么路径名将呈现path1path2这种形式。首先回到根目录，然后一级一级的到达目的。在笔者的OBEX类当中，可以看到BacktoRoot这个过程。其作用就是把当前程序路径切换到根目录以免引起混乱。
l 取得目录信息

要取得一个目录的文件信息可以用发送Get指令。该Get指令需要一个TypeHeader，其值为x-obex/folder-listing

随后服务端会返回一个xml文件，包含了整个目录的信息。例如子目录名、文件名、大小、最后修改时间。再通过xml解析后就会得到目录信息。
l 创建目录

当SetPath的Flags的Bit1设为1并且无法找到目录时，服务端会创建这个目录并进入。

请注意，再次使用SetPath并将Flags的Bit0设为1返回上层目录，否则容易引起混乱。
l 上传下载文件

使用Put和Get命令可以实现上传和下载文件。注意所有的文件操作都在SetPath所指定的目录下进行。

使用Put命令实现上传时，至少需要提供NameHeader，BodyHeader，一般还要提供LengthHeader，DateTimeHeader。大文件传输需要正确处理BodyHeader，不能超出了服务端最大所能接收的Packet的大小，否则会出现错误。

需要注意的是如果当前文件存在，那么Put命令不会覆盖已有的文件而是追加，导致错误。传输文件之前需要首先删除同名文件。

使用Get命令实现下载时，只需要提供NameHeader即可。具体例子参考上一节。
l 删除文件或空目录

使用Put命令，其NameHeader指定为文件名或目录名并将BodyHeader设置为空即可。

非空目录无法删除，会返回错误。


西门子手机还有移动、拷贝等功能，具体实现方法请参阅OBEX源代码。
