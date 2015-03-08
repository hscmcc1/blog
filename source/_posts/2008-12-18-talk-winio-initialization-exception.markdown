---
author: hesicong
comments: true
date: 2008-12-18 14:49:28+00:00
layout: post
slug: talk-winio-initialization-exception
title: 再谈WinIO初始化异常
wordpress_id: 1473
categories:
- 其他
tags:
- c
- winio
---

前段时间WinIO在我的新项目中总是初始化失败，有时候又是好好的，很让人费解。修改了源代码显示了很多调试信息后，也没有什么太多的收获。由于我们的工控卡必须要用这个库，所以没办法，只得停下脚步，细细研究一下问题所在。

初始化的时候调用的是InitializeWinIo()函数:

``` c
bool _stdcall InitializeWinIo()
{
bool bResult;
DWORD dwBytesReturned;`

IsNT = IsWinNT();

if (IsNT)
{
hDriver = CreateFile("\\.\WINIO",
GENERIC_READ | GENERIC_WRITE,
0,
NULL,
OPEN_EXISTING,
FILE_ATTRIBUTE_NORMAL,
NULL);

// If the driver is not running, install it

if (hDriver == INVALID_HANDLE_VALUE)
{
GetDriverPath();

bResult = InstallWinIoDriver(szWinIoDriverPath, true);

if (!bResult)
return false;

bResult = StartWinIoDriver();

if (!bResult)
return false;

hDriver = CreateFile("\\.\WINIO",
GENERIC_READ | GENERIC_WRITE,
0,
NULL,
OPEN_EXISTING,
FILE_ATTRIBUTE_NORMAL,
NULL);

if (hDriver == INVALID_HANDLE_VALUE)
return false;
}

// Enable I/O port access for this process

if (!DeviceIoControl(hDriver, IOCTL_WINIO_ENABLEDIRECTIO, NULL,
0, NULL, 0, &dwBytesReturned, NULL))
return false;

}
else
{
VxDCall = (DWORD (WINAPI *)(DWORD,DWORD,DWORD))GetK32ProcAddress(1);

hDriver = CreateFile("\\.\WINIO.VXD", 0, 0, 0, CREATE_NEW, FILE_FLAG_DELETE_ON_CLOSE, 0);

if (hDriver == INVALID_HANDLE_VALUE)
return false;
}

IsWinIoInitialized = true;

return true;
}
```

函数首先查看是不是NT系统，如果是，就创建设备驱动的handle，如果没有的话，就安装一个。有的话就直接使用。

仔细看看这个函数值后就知道其实引起初始化失败的原因很多。我遇到的问题就是有时候异常退出了，第二次就无法正常启动WinIO了，只能按照这样的方式来进行：

InitWinIO->成功->程序异常退出->InitWinIO->失败->ShutdownWinIO->InitWinIO->成功

也就是说重新启动两次程序才行。由于异常退出保留在内存的驱动第二次InitWinIO会失败，然后ShutdownWinIO就会卸载这个驱动，再进行InitWinIO就正确了。

那么直接在应用程序启动前无论怎么就运行ShutdownWinIO先关闭一下可以么？看看代码似乎是可以的：

``` c
void _stdcall ShutdownWinIo()
{
DWORD dwBytesReturned;`

if (IsNT)
{
if (hDriver != INVALID_HANDLE_VALUE)
{
// Disable I/O port access

DeviceIoControl(hDriver, IOCTL_WINIO_DISABLEDIRECTIO, NULL,
0, NULL, 0, &dwBytesReturned, NULL);

CloseHandle(hDriver);

}

RemoveWinIoDriver();
}
else
CloseHandle(hDriver);

IsWinIoInitialized = false;
}
```

我测试过，直接在程序一开始就执行ShutdownWinIo()然后在初始化WinIO，一样可能提示出错。什么原因呢？

其实，WinIO的ShutdownWinIo这里有一个bug：

```
if(IsNT)
```

如果你没有调用过InitializeWinIo()的话，全局变量IsNT是为false的，他就没有正确的执行nt平台的代码。这就解释了为什么没有关闭成功。

所以，找到了问题所在，有两种解决方法：

* 将IsNT替换为IsWinNT()的函数调用
* 在InitWinIO里面if(IsNT)以后，执行一次ShutdownWinIO即可。

这样，我所遇到的初始化失败问题终于得到了解决：)
