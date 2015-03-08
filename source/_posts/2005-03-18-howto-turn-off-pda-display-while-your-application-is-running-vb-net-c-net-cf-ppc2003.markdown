---
author: hesicong
comments: true
date: 2005-03-18 15:57:40+00:00
layout: post
slug: howto-turn-off-pda-display-while-your-application-is-running-vb-net-c-net-cf-ppc2003
title: HOWTO:Turn Off PDA Display while your application is running.(VB.Net/c#+.Net
  CF+PPC2003)
wordpress_id: 1229
categories:
- 其他
tags:
- .net
- pda
---

Core code for VB.NET:

```
Namespace PDA
Public Class Video
Private Const SETPOWERMANAGEMENT As Int32 = 6147
Declare Function ExtEscapeSet Lib "coredll" Alias "ExtEscape" (ByVal hdc As IntPtr, _
ByVal nEscape As Int32, _
ByVal cbInput As Int32, _
ByVal plszInData As Byte(), _
ByVal cbOutput As Int32, _
ByVal lpszOutData As IntPtr) As Int32
Declare Function GetDC Lib "coredll" (ByVal hwnd As IntPtr) As IntPtr
Public Enum VideoPowerState As Integer
VideoPowerOn = 1
VideoPowerStandBy
VideoPowerSuspend
VideoPowerOff
End Enum
Public Shared Sub PowerOff()
Dim hdc As IntPtr = GetDC(IntPtr.Zero)
Dim vpm() As Byte = {12, 0, 0, 0, 1, 0, 0, 0, VideoPowerState.VideoPowerOff, 0, 0, 0, 0}
ExtEscapeSet(hdc, SETPOWERMANAGEMENT, 12, vpm, 0, IntPtr.Zero)
End Sub
Public Shared Sub PowerOn()
Dim hdc As IntPtr = GetDC(IntPtr.Zero)
Dim vpm() As Byte = {12, 0, 0, 0, 1, 0, 0, 0, VideoPowerState.VideoPowerOn, 0, 0, 0, 0}
ExtEscapeSet(hdc, SETPOWERMANAGEMENT, 12, vpm, 0, IntPtr.Zero)
End Sub
End Class
End Namespace
Core code for C#
using System;
using System.Runtime.InteropServices;
namespace OpenNETCF
{
/// <summary>
/// Summary description for Video.
/// </summary>
public class Video
{
// GDI Escapes for ExtEscape()
private const uint QUERYESCSUPPORT = 8;
// The following are unique to CE
private const uint GETVFRAMEPHYSICAL = 6144;
private const uint GETVFRAMELEN = 6145;
private const uint DBGDRIVERSTAT = 6146;
private const uint SETPOWERMANAGEMENT = 6147;
private const uint GETPOWERMANAGEMENT = 6148;
public static void PowerOff()
{
IntPtr hdc = GetDC(IntPtr.Zero);
uint func = SETPOWERMANAGEMENT;
uint size = 12;
byte[] vpm = new byte[size];

//structure size
BitConverter.GetBytes(size).CopyTo(vpm, 0);
//dpms version
BitConverter.GetBytes(0x0001).CopyTo(vpm, 4);
//power state
BitConverter.GetBytes((uint)VideoPowerState.VideoPowerOff).CopyTo(vpm, 8);
ExtEscapeSet(hdc, SETPOWERMANAGEMENT, size, vpm, 0, IntPtr.Zero);
}
public static void PowerOn()
{
IntPtr hdc = GetDC(IntPtr.Zero);
uint size = 12;
byte[] vpm = new byte[size];

//structure size
BitConverter.GetBytes(size).CopyTo(vpm, 0);
//dpms version
BitConverter.GetBytes(0x0001).CopyTo(vpm, 4);
//power state
BitConverter.GetBytes((uint)VideoPowerState.VideoPowerOn).CopyTo(vpm, 8);
ExtEscapeSet(hdc, SETPOWERMANAGEMENT, size, vpm, 0, IntPtr.Zero);
}
[DllImport("coredll", EntryPoint="ExtEscape")]
private static extern int ExtEscapeSet(
IntPtr hdc,
uint nEscape,
uint cbInput,
byte[] lpszInData,
int cbOutput,
IntPtr lpszOutData
);
[DllImport("coredll")]
private static extern IntPtr GetDC(IntPtr hwnd);
}

public enum VideoPowerState : uint
{
VideoPowerOn = 1,
VideoPowerStandBy,
VideoPowerSuspend,
VideoPowerOff

}
```
