---
author: hesicong
comments: true
date: 2005-07-15 16:08:32+00:00
layout: post
slug: ii-thesaurus-easily-back-word-converter-source-code
title: 轻轻松松背单词II 词库转换器(源代码)
wordpress_id: 1246
categories:
- 其他
---


原理很简单，看看就懂：)

```
Imports System.IO
Imports System.Text
Module BDCWordConverter
Sub main()
Dim dir As New DirectoryInfo("D:bdcWord")
Dim fi As FileInfo() = dir.GetFiles("*.gds")
For Each f As FileInfo In fi
Dim fs As New FileStream(f.FullName, FileMode.Open)
Dim br As New BinaryReader(fs)
fs.Position = 12
Dim b01 As Byte() = br.ReadBytes(20)
ReDim Preserve b01(27)
br.BaseStream.Position = 50
Dim b02 As Byte() = br.ReadBytes(8)
b02.CopyTo(b01, 20)
Console.WriteLine(f.FullName)
Dim fileName As String = New StringBuilder(Encoding.GetEncoding("GB2312").GetChars(b01)).ToString.TrimEnd(CChar(" "))
Console.WriteLine("Processing {0}", fileName)
Dim fw As New StreamWriter("D:bdc word" & fileName.TrimEnd(Chr(0)) & ".txt")
Dim startPos As Integer = 290
Dim offWord As Integer = 30
Dim offPun As Integer = 30
Dim offMean As Integer = 40
Dim offCourse As Integer = 28
br.BaseStream.Position = 290
Dim Word As String
Dim Pun As String
Dim Mean As String
Do Until br.PeekChar = -1
Dim b1 As Byte() = br.ReadBytes(offWord)
Word = New ASCIIEncoding().GetChars(b1)
Dim b2 As Byte() = br.ReadBytes(offPun)
Pun = New ASCIIEncoding().GetChars(b2)
Dim b3 As Byte() = br.ReadBytes(offMean)
Mean = New StringBuilder(Encoding.GetEncoding("GB2312").GetChars(b3)).ToString
br.ReadBytes(offCourse)
fw.WriteLine("""{0}"",""{1}"",""{2}""", Word.TrimEnd(CChar(" ")), Pun.TrimEnd(CChar(" ")), Mean.TrimEnd(CChar(" ")))
Loop
fs.Flush()
fs.Close()
Next
End Sub

End Module
```
