---
author: hesicong
comments: true
date: 2008-12-11 03:47:41+00:00
layout: post
slug: winio-initialization-failed-for-several-reasons
title: WinIO初始化失败的几个原因
wordpress_id: 1144
categories:
- 其他
tags:
- winio
---

WinIO是一个能够打开一些操作系统IO特权操作的一个库，简单的来说它加载了一些驱动。通过加载的驱动可以直接的访问端口。在工控上，我们可以利用这个库直接操作IO卡的地址。例如我最近在做的一个数控钻铣床的IO卡和运动控制卡就是很老的一个卡，在WIN98下面工作很正常，但是在NT核心上就会出现非法指令调用的问题。这些非法指令来自于底层对IO卡和运动控制卡的直接地址访问。在98时代，这些操作都没有受到保护的，在NT核心下就会出现保护问题。经试验，经过WinIO初始化载入驱动以后再进行IO卡直接访问，很成功。

但应用的时候，就出现了一些莫名其妙的问题。应用WinIO只需要调用两个函数InitializeWinIo()，和最后的ShutdownWinIo()即可。InitializeWinIo()将会返回一个bool值指示初始化结果。就是这个函数造成了许多困扰。

第一次困扰是在一次调试中，经常初始化失败，一旦成功以后就总是成功的。刚开始以为是InitializeWinIo()以后没有ShutdownWinIoI()造成的，后来看了WinIO的C Example证明只写InitializeWinIo()一样能够进程一旦结束，由InitializeWinIo初始化的资源自然就结束了。所以不存在上次运行影响这次运行的事情。后来突然发现，WinIO相关的dll,vxd,sys竟然是绿色的。绿色在WindowsXP系统里面代表了文件是被EFS加密的。我为了工程的保密，把所有的工程目录都进行了EFS加密。EFS加密会影响磁盘性能，原因就在于其加解密过程。但是这里很奇怪，可能是间歇性的EFS解密速度没有跟上WinIO中加载驱动的速度，造成读取的sys和vxd设备驱动是混乱的，最终导致加载失败。将EFS加密取消，问题解决。

第二次困找在我用C#写了一个dllimport，然后进行调用，结果，总是返回false。很疑惑，WinIO相关的文件都放到一起的，怎么还是这样的呢？VS2005单元测试里也会失败。究其原因还是路径的问题造成。分析WinIO的源代码，可以发现InitializeWinIo()会调用一个GetDriverPath这个函数：

```
bool GetDriverPath()
{
PSTR pszSlash;

if (!GetModuleFileName(GetModuleHandle(NULL), szWinIoDriverPath, sizeof(szWinIoDriverPath)))
return false;

pszSlash = strrchr(szWinIoDriverPath, '\');

if (pszSlash)
pszSlash[1] = 0;
else
return false;

strcat(szWinIoDriverPath, "winio.sys");

return true;
}
```

这里面已经很清楚的知道了什么情况下会false了。注意winio.sys存放的位置问题就能使之初始化正常。

其实还可以更加详细的打印出InitializeWinIo()中每步的执行过程，这样更容易判断是哪个地方出现了问题。

就先写到这里吧，WinIO是个很好很强大，很黄很暴力的一个库～～～
