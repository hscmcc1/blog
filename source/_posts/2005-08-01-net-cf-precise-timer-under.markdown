---
author: hesicong
comments: true
date: 2005-08-01 16:14:01+00:00
layout: post
slug: net-cf-precise-timer-under
title: .Net CF下精确的计时器
wordpress_id: 1254
categories:
- 其他
tags:
- .net
- pda
---

.Net CF下精确的计时器

用法：

```
Dim t as New AtomicCF.Timer
t.start()
....'Some functions here
Dim TimeLapsed as Long = t.stop()
```


代码：

```
Imports System.Runtime.InteropServices
Namespace AtomicCF

Public Class Timer
<DllImport("coredll.dll", EntryPoint:="QueryPerformanceCounter")> _
Public Shared Function QueryPerformanceCounter(ByRef perfCounter As Long) As Integer
End Function

<DllImport("coredll.dll", EntryPoint:="QueryPerformanceFrequency")> _
Public Shared Function QueryPerformanceFrequency(ByRef frequency As Long) As Integer
End Function

Private m_frequency As Int64
Private m_start As Int64

Public Sub New()
If QueryPerformanceFrequency(m_frequency) = 0 Then
Throw New ApplicationException
End If
'Convert to ms.
m_frequency = CLng(m_frequency / 1000)
End Sub

Public Sub Start()
If QueryPerformanceCounter(m_start) = 0 Then
Throw New ApplicationException
End If
End Sub


            Public Function [Stop]() As Int64
                Dim lStop As Int64 = 0
                If QueryPerformanceCounter(lStop) = 0 Then
                    Throw New ApplicationException
                End If
                Return CLng((lStop - m_start) / m_frequency)
            End Function
        End Class
    End Namespace

```