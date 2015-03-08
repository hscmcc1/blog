---
author: hesicong
comments: true
date: 2004-07-13 15:04:24+00:00
layout: post
slug: test-drive-the-speed-with-vb-net
title: 用VB.NET测试硬盘速度
wordpress_id: 1170
categories:
- 硬件
tags:
- .net
---

前日在用SiSoftware Sandra 2004测试硬盘性能时突发其想，用自己熟悉的VB.NET测试行不行呢？具体怎么做呢？

我们最感兴趣的是硬盘在最大负荷下持续的读取和写入速度。为了能够比较准确的测出平均速度，我决定采用先写入一个1GB的文件再读取出来的办法。考虑到不要让更多的任务花在循环上，我首先建立起一个足够大的缓冲区，然后往磁盘写入这个缓冲的内容，从而使硬盘达到最大的负荷。考虑到Windows的读取机制，硬盘测试不太准确，此程序的读取部分只能在第一次运行时使用，运行次数越多测试也不准确，而写入测试多次运行以后依然能够保持准确性。现在就开始动手。

在VB.NET中创建了一个控制台工程TestHarddisk，然后在Sub Main中写入下列程序。


```
Sub Main()
Dim I As Int32
Dim f As New FileStream("E:BigFile.big", FileMode.Create)
Dim fw As New BinaryWriter(f)
Dim fr As New BinaryReader(f)
Dim Size As Int32 = 1024 * 1024 * 1024 - 1? 'File size = 1GB
Dim bufSize As Int32 = 30 * 1024 * 1024? 'Buffer Size = 30MB
Dim jLast As Int32 = bufSize - 1
Dim j As Int32
Dim Bytes(bufSize) As Byte
Dim StartWrite As Date = Date.Now
Console.WriteLine("Write Start at {0}", StartWrite)
Console.WriteLine("Creating...")
For I = 0 To Size Step bufSize '1GB
fw.Write(Bytes)
Next
Dim EndWrite As Date = Date.Now
Dim TimePassed As TimeSpan = EndWrite.Subtract(StartWrite)
Console.WriteLine("Write End at {0}", EndWrite)
Console.WriteLine("Time passed:{0}", TimePassed)
Console.WriteLine("Speed:{0}", 1000 / TimePassed.TotalSeconds)
fw.Flush()
Dim StartRead As Date = Date.Now
Console.WriteLine("Read Start at {0}", StartRead)
Console.WriteLine("Reading")
For I = 0 To Size Step bufSize
Bytes = fr.ReadBytes(bufSize)
Next
Dim EndRead As Date = Date.Now
TimePassed = EndRead.Subtract(StartRead)
Console.WriteLine("Read End at {0}", EndRead)
Console.WriteLine("Time passed:{0}", TimePassed)
Console.WriteLine("Read speed:{0}", 1000 / TimePassed.TotalSeconds)
Console.ReadLine()
fw.Close()
End Sub
```

现在测试。

硬件配置：

Athlon 2500+(running at 1.8G)

EPOX 8RDA3+ nForce2主板

512DDR 400(running at 400MHz)

ATA100 Seagate 7200.7 80GB 2MB

软件配置：

Windows 2003 Server Standard Edition

全套最新的WHQL驱动程序

Visual Studio 2003，编译选择Realse模式，打开所有优化选项。

第一次运行测试得写入速度43MB/s，第二次测得42MB/s，第三次41MB/s与Sisoftware测试得的结果43MB/s相差不大，达到了理想的效果。然而读取测试就变态了，第一次64MB/s，与Sissoftware测试多了5MB/s左右，第二次达到了1096MB/s，第三次 1123MB/s，这和Windows的磁盘缓冲机制有关，看来作用还是蛮大的，当然，建议读取测试在重起电脑以后进行。
