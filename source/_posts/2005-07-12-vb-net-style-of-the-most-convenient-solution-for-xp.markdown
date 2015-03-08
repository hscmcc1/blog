---
author: hesicong
comments: true
date: 2005-07-12 16:04:18+00:00
layout: post
slug: vb-net-style-of-the-most-convenient-solution-for-xp
title: VB.NET里最方便的XP风格解决方案
wordpress_id: 1238
categories:
- 其他
tags:
- .net
---


将以下代码添加到InitializeComponent()之后

```
On Error Resume Next
Dim y As Integer
Dim AppName As String
Dim ManFileName As String
Dim FullAppExeNameAndPath As String
FullAppExeNameAndPath = Application.ExecutablePath
y = Application.StartupPath.Length
' y = FullAppExeNameAndPath.LastIndexOf("")
y = y + 1
AppName = FullAppExeNameAndPath.Substring(y, FullAppExeNameAndPath.Length - y)
ManFileName = AppName & ".manifest"

If System.IO.File.Exists(ManFileName) = False Then
FileOpen(1, ManFileName, OpenMode.Binary)
FilePut(1, "<?xml version='1.0' encoding='UTF-8' standalone='yes'?>" & Environment.NewLine)
FilePut(1, "<assembly xmlns='urn:schemas-microsoft-com:asm.v1' manifestVersion='1.0'>" & Environment.NewLine)
FilePut(1, "<assemblyIdentity version='1.0.0.0' processorArchitecture='X86' name='zx.exe' type='win32' />" & Environment.NewLine)
FilePut(1, "<description>zxapplication</description>" & Environment.NewLine)
FilePut(1, "<dependency>" & Environment.NewLine)
FilePut(1, "<dependentAssembly>" & Environment.NewLine)
FilePut(1, "<assemblyIdentity type='win32' name='Microsoft.Windows.Common-Controls' version='6.0.0.0' processorArchitecture='X86' publicKeyToken='6595b64144ccf1df' language='*' />" & Environment.NewLine)
FilePut(1, "</dependentAssembly>" & Environment.NewLine)
FilePut(1, "</dependency>" & Environment.NewLine)
FilePut(1, "</assembly>" & Environment.NewLine)
FileClose(1)
End If
```