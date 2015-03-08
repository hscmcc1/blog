---
author: hesicong
comments: true
date: 2005-08-17 09:45:05+00:00
layout: post
slug: net-compact-framework-in-decoding-gb2312
title: .NET Compact Framework中解码GB2312
wordpress_id: 1377
categories:
- 其他
tags:
- .net
- pda
---


昨天在制作“掌上IP通”调用纯真IP数据库的时候，遇到了GB2312解码的问题。

我想，在.NET平台上本来可以用 System.Text.Encoding.GetEncoding("GB2312")得到GB2312的解码器的，在.NET Compact Framework中也可以使用这个方法。查阅MSDN，也确实能够支持。但十分遗憾的是，当在PDA上执行此条语句的时候，返回的却是null。如果直 接使用System.Text.Encoding.GetEncoding("GB2312").GetString方法的话，会抛出一个 PlatformNotSupport异常。

后查询到MSDN关于这个问题的解答(详见[http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dv_evtuv/html/etcondeviceprojectglobalization.asp](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dv_evtuv/html/etcondeviceprojectglobalization.asp))
Encoding
The .NET Compact Framework supports character encoding on all devices: Unicode (BE and LE), UTF8, UTF7, and ASCII.

There is limited support for code page encoding and only if the encoding is recognized by the operating system of the device.

The .NET Compact Framework throws a PlatformNotSupportedException if the a required encoding is not available on the device.

If the optional component Mlang.dll is on the device, the following code pages are supported: CP 51932 (EUC-JP), CP 50220 (ISO2022JP), and CP 50221 (cslSO2022JP).


也就是说GB2312不受.NET Compact Framework的支持。

很无赖，我们无法用常规的方法来解码GB2312。在此搜索有关GB2312的文章，发现原来GB2312有一个对照表，可以和Unicode进行双向的转换。后下载到一个GB2312.TXT的对照表(请见[[download id="28"]](http://www.hesicong.com/Docum/gb2312.txt))，对照词表可以将GB2312转换成Unicode然后就可以解码了。

根据GB2312.txt中所述，需要将实际的编码减去0x8080，然后再查表。

例如“南”的GB2312编码为：0xC4CF，查表的时候减去0x8080，得到0x444F，查表得到Unicode码：0x5357。此后利用System.Text.Encoding.Unicode.GetChars()就可以得到相应的字符。

虽然转了一个弯，但是还是达到了目的。

为此，我写了一个GB2312 To Unicode Conver Table 的控制台程序，以便将GB2312.TXT转换成一个索引供查询。编码好的文件下载：[](http://www.hesicong.com/Docum/gb2312.dat)[download id="29"]

代码如下：

```
Imports System.Text
Imports System.IO
Module Module1Module Module1

 Sub Main()Sub Main()
 Dim sr As New StreamReader("D:gb2312.txt")
 Dim fs As New FileStream("D:gb2312.dat", FileMode.Create)
 Dim sw As New BinaryWriter(fs)
 For i As Integer = 1 To 70
 sr.ReadLine()
 Next
 Do Until sr.EndOfStream
 Dim ss As String() = sr.ReadLine.Split
 Dim HEX As Short = CShort(Val(ss(1).Replace("0x", "&H")))
 Dim offset As Integer = (CInt(Val(ss(0).Replace("0x", "&H"))) - &H2121) * 2
 sw.BaseStream.Position = offset
 sw.Write(HEX)
 Loop
 sw.Close()
 End Sub

End Module
```


使用的时候将欲解码的GB2312编码减去0x8080，再减去0x2121(偏移)，也就是说减去0xA1A1，然后将 FileStream.Position指向所得到的结果的2倍(两个字节一个码)并读取一个Int16就可以得到对应的Unicode码。下面是我在掌 心IP通中的一段代码。

```
 public class GB2312ToUnicode
 {
 private FileStream fs;
 private BinaryReader br;
 public GB2312ToUnicode(string index)
 {
 fs=new FileStream(index, FileMode.Open);
 br = new BinaryReader(fs);
 }

 public char Convert(byte[] b)
 {
 int index = ((b[0] << 8) + b[1] - 0xa1a1)*2;
 fs.Position = index;
 int tmp = (int)br.ReadInt16() & 0xFFFF;
 return System.Convert.ToChar(tmp);
 }

 ~GB2312ToUnicode()
 {
 br.Close();
 }

 }
```

这样就方便的得到了GB2312的解码器。

另外，如果打开GB2312.DAT文件可以看到，数据中有很多的0。本来可以通过重新计算偏移的方法来压缩数据，但是考虑到PocketPC上面的速度，尤其是要解码大量文本的时候，就只好用空间换取时间了。
