---
author: hesicong
comments: true
date: 2004-07-12 14:52:12+00:00
layout: post
slug: how-vb-net-download-to-complete-web-page
title: 如何用vb.net下载到完整的web页
wordpress_id: 1163
categories:
- 其他
tags:
- .net
---

在网络上找了很多关于网页下载的程序但都不能完整地得到web页的内容，以下这个函数解决了这个问题。


```
    Private Function GetSource(ByVal url As String) As String
    Try
    Dim httpReq As System.Net.HttpWebRequest　'HttpWebRequest 类对 WebRequest 中定义的属性和方法提供支持'，也对使用户能够直接与使用 HTTP 的服务器交互的附加属性和方法提供支持。
    Dim httpResp As System.Net.HttpWebResponse　' HttpWebResponse 类用于生成发送 HTTP 请求和接收 HTTP 响'应的 HTTP 独立客户端应用程序。
    Dim httpURL As New System.Uri(url)
    httpReq = CType(WebRequest.Create(httpURL), HttpWebRequest)
    httpReq.Method = "GET"
    httpResp = CType(httpReq.GetResponse(), HttpWebResponse)
    Dim reader As StreamReader = _
    New StreamReader(httpResp.GetResponseStream, System.Text.Encoding.GetEncoding("GB2312")) '如是中文，要设置编码格式为“GB2312”。
    Dim respHTML As String = reader.ReadToEnd()　'respHTML就是网页源代码
    Return respHTML
    httpResp.Close()
    Catch e As Exception
    Console.WriteLine("GetSource出现问题:{0},{1}", e.Message, url)
    End Try
    End Function
```