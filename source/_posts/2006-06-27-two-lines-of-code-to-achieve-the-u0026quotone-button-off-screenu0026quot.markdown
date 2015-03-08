---
author: hesicong
comments: true
date: 2006-06-27 10:13:02+00:00
layout: post
slug: two-lines-of-code-to-achieve-the-u0026quotone-button-off-screenu0026quot
title: 两行代码实现“一键关屏”
wordpress_id: 1442
categories:
- 其他
tags:
- .net
---


有时候暂时离开一下电脑，不关显示器呢觉得又浪费电了，不手动关呢又要等一段时候屏保才启动，还是要浪费少许电。还是自己动手写一个算了。查阅了相关的资料，发现两行代码足以。

在vs2005里面建立一个vc++工程，创建一个main.cpp，然后拷贝一下代码进去。
1![](http://www.cnblogs.com/Images/OutliningIndicators/None.gif)#include <windows.h>
2![](http://www.cnblogs.com/Images/OutliningIndicators/None.gif)int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow)
3![](http://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)![](http://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](http://www.cnblogs.com/Images/dot.gif){
4![](http://www.cnblogs.com/Images/OutliningIndicators/InBlock.gif) // Eliminate user's interaction for 500 ms
5![](http://www.cnblogs.com/Images/OutliningIndicators/InBlock.gif) Sleep(500);
6![](http://www.cnblogs.com/Images/OutliningIndicators/InBlock.gif)
7![](http://www.cnblogs.com/Images/OutliningIndicators/InBlock.gif) // Turn off monitor
8![](http://www.cnblogs.com/Images/OutliningIndicators/InBlock.gif) SendMessage(HWND_BROADCAST, WM_SYSCOMMAND, SC_MONITORPOWER, (LPARAM) 2);
9![](http://www.cnblogs.com/Images/OutliningIndicators/InBlock.gif)
10![](http://www.cnblogs.com/Images/OutliningIndicators/InBlock.gif) return 0;
11![](http://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockEnd.gif)}
12![](http://www.cnblogs.com/Images/OutliningIndicators/None.gif)
13![](http://www.cnblogs.com/Images/OutliningIndicators/None.gif)

编译以后生成一个文件，然后在桌面上创建一个它的快捷方式。

编辑快捷方式的属性，找到“快捷键”这一栏，设置为你需要的快捷键。我这里设置为Ctrl+Shift+Alt+F12，保存，这样需要关屏的时候只需要按以上快捷键就可以了。
![](http://images.cnblogs.com/cnblogs_com/hesicong/untitled.JPG)


如果你还嫌麻烦(真懒……)，那么下载这个文件就行了：
[](http://hesicong.cnblogs.com/Files/hesicong/OneClickDisplayTurnoff.rar)[download id="30"]
