---
author: hesicong
comments: true
date: 2004-09-28 15:13:54+00:00
layout: post
slug: encode-charactors-to-7bitcode-or-ucs2-pdu-string-vb-net
title: Encode Charactors to 7BitCode or UCS2 PDU string (vb.net)
wordpress_id: 1175
categories:
- 其他
tags:
- .net
- pdu
---

Encode Charactors to 7BitCode or UCS2 PDU string (vb.net)

```
    Public Function Encode7Bit(ByVal Content As String) As String
    'Prepare
    Dim CharArray As Char() = Content.ToCharArray
    Dim c As Char
    Dim t As String
    For Each c In CharArray
    t = CharTo7Bits(c) + t
    Next
    'Add "0"
    Dim i As Integer
    If (t.Length Mod 8) <> 0 Then
    For i = 1 To 8 - (t.Length Mod 8)
    t = "0" + t
    Next
    End If
    'Split into 8bits
    Dim result As String
    For i = t.Length - 8 To 0 Step -8
    result = result + BitsToHex(Mid(t, i + 1, 8))
    Next
    Return result
    End Function

    Private Function BitsToHex(ByVal Bits As String) As String
    'Convert 8Bits to Hex String
    Dim i, v As Integer
    For i = 0 To Bits.Length - 1
    v = v + Val(Mid(Bits, i + 1, 1)) * 2 ^ (7 - i)
    Next
    Dim result As String
    result = Format(v, "X")
    If result.Length = 1 Then
    result = "0" + result
    End If
    Return result
    End Function

    Private Function CharTo7Bits(ByVal c As Char) As String
    Dim Result As String
    Dim i As Integer
    For i = 0 To 6
    If (Asc(c) And 2 ^ i) > 0 Then
    Result = "1" + Result
    Else
    Result = "0" + Result
    End If
    Next
    Return Result
    End Function

    Private Function EncodeUCS2(ByVal Content As String) As String
    Dim i, j, v As Integer
    Dim Result, t As String
    For i = 1 To Content.Length Step 4
    v = AscW(Mid(Content, i, 4))
    t = Format(v, "X")
    For j = 1 To 4 - t.Length
    t = "0" & t
    Next
    Result += t
    Next
    Return Result
    End Function
```
