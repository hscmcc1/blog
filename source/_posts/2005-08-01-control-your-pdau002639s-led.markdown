---
author: hesicong
comments: true
date: 2005-08-01 16:12:58+00:00
layout: post
slug: control-your-pdau002639s-led
title: Control your PDA's LED
wordpress_id: 1252
categories:
- 其他
tags:
- .net
- pda
---

I wrote a VB.NET class named LED to control PDA's LED.

我写了一个名为LED的类来控制PDA的LED灯。
Please see [http://www.pocketpcdn.com/articles/led.html](http://www.pocketpcdn.com/articles/led.html) first to help understand the core of this class.

请首先阅读[http://www.pocketpcdn.com/articles/led.html](http://www.pocketpcdn.com/articles/led.html)，这将有利于你理解这个类
And here is the class:

以下是这个类：

```
Imports System.runtime.InteropServices

Public Class LED
Private Structure NLED_SETTINGS_INFO
Public LedNum As UInt32
Public OffOnBlink As UInt32
Public TotalCycleTime As Integer
Public OnTime As Integer
Public OffTime As Integer
Public MetaCycleOn As Integer
Public MetaCycleOff As Integer
End Structure

Private Structure NLED_COUNT_INFO
Public cLeds As Integer
End Structure

Private Const NLED_COUNT_INFO_ID = 0
Private Const NLED_SETTINGS_INFO_ID = 2

Private Declare Function NLedGetDeviceInfo Lib "coredll.dll" (ByVal nID As Integer, ByRef pOutput As NLED_COUNT_INFO) As Boolean
Private Declare Function NLedSetDevice Lib "coredll.dll" (ByVal nID As Integer, ByRef pOutput As NLED_SETTINGS_INFO) As Boolean

Public Enum Status
OFF
[ON]
BLINK
End Enum

Public Function GetLedCount() As Integer
Dim nci As NLED_COUNT_INFO
Dim wCount As Integer = 0
If NLedGetDeviceInfo(NLED_COUNT_INFO_ID, nci) Then
wCount = CInt(nci.cLeds)
End If
Return wCount
End Function

Public Sub SetLedStatus(ByVal wLed As Integer, ByVal wStatus As Status)
Dim nsi As NLED_SETTINGS_INFO
nsi.LedNum = System.Convert.ToUInt32(wLed)
nsi.OffOnBlink = System.Convert.ToUInt32(wStatus)
NLedSetDevice(NLED_SETTINGS_INFO_ID, nsi)
End Sub

End Class

```

And the test form is here:

这儿是用于测试窗体:

```
Imports System
Imports System.Collections
Imports System.Text
Public Class Form1
Inherits System.Windows.Forms.Form
Friend WithEvents MenuItem1 As System.Windows.Forms.MenuItem
Friend WithEvents MenuItem2 As System.Windows.Forms.MenuItem
Friend WithEvents MainMenu1 As System.Windows.Forms.MainMenu

#Region " Windows 窗体设计器生成的代码 "

Public Sub New()
MyBase.New()

'该调用是 Windows 窗体设计器所必需的。
InitializeComponent()

'在 InitializeComponent() 调用之后添加任何初始化

End Sub

'窗体重写 dispose 以清理组件列表。
Protected Overloads Overrides Sub Dispose(ByVal disposing As Boolean)
MyBase.Dispose(disposing)
End Sub

'注意: 以下过程是 Windows 窗体设计器所必需的
'可以使用 Windows 窗体设计器修改此过程。
'不要使用代码编辑器修改它。
Friend WithEvents Label1 As System.Windows.Forms.Label
Friend WithEvents Label2 As System.Windows.Forms.Label
Friend WithEvents Panel1 As System.Windows.Forms.Panel
Friend WithEvents rbLED As System.Windows.Forms.RadioButton
Friend WithEvents rbOn As System.Windows.Forms.RadioButton
Friend WithEvents rbOff As System.Windows.Forms.RadioButton
Friend WithEvents rbBlink As System.Windows.Forms.RadioButton
Private Sub InitializeComponent()
Me.MainMenu1 = New System.Windows.Forms.MainMenu
Me.MenuItem1 = New System.Windows.Forms.MenuItem
Me.MenuItem2 = New System.Windows.Forms.MenuItem
Me.rbLED = New System.Windows.Forms.RadioButton
Me.Label1 = New System.Windows.Forms.Label
Me.Label2 = New System.Windows.Forms.Label
Me.Panel1 = New System.Windows.Forms.Panel
Me.rbOn = New System.Windows.Forms.RadioButton
Me.rbOff = New System.Windows.Forms.RadioButton
Me.rbBlink = New System.Windows.Forms.RadioButton
'
'MainMenu1
'
Me.MainMenu1.MenuItems.Add(Me.MenuItem1)
'
'MenuItem1
'
Me.MenuItem1.MenuItems.Add(Me.MenuItem2)
Me.MenuItem1.Text = "菜单"
'
'MenuItem2
'
Me.MenuItem2.Text = "退出"
'
'rbLED
'
Me.rbLED.Checked = True
Me.rbLED.Location = New System.Drawing.Point(24, 48)
Me.rbLED.Size = New System.Drawing.Size(60, 16)
Me.rbLED.Text = "0"
'
'Label1
'
Me.Label1.Location = New System.Drawing.Point(12, 24)
Me.Label1.Size = New System.Drawing.Size(80, 16)
Me.Label1.Text = "LED No."
'
'Label2
'
Me.Label2.Location = New System.Drawing.Point(120, 24)
Me.Label2.Size = New System.Drawing.Size(80, 16)
Me.Label2.Text = "LED Status"
'
'Panel1
'
Me.Panel1.Controls.Add(Me.rbOn)
Me.Panel1.Controls.Add(Me.rbOff)
Me.Panel1.Controls.Add(Me.rbBlink)
Me.Panel1.Location = New System.Drawing.Point(136, 48)
Me.Panel1.Size = New System.Drawing.Size(76, 96)
'
'rbOn
'
Me.rbOn.Location = New System.Drawing.Point(6, 36)
Me.rbOn.Size = New System.Drawing.Size(38, 20)
Me.rbOn.Text = "On"
'
'rbOff
'
Me.rbOff.Checked = True
Me.rbOff.Location = New System.Drawing.Point(6, 8)
Me.rbOff.Size = New System.Drawing.Size(38, 20)
Me.rbOff.Text = "Off"
'
'rbBlink
'
Me.rbBlink.Location = New System.Drawing.Point(8, 64)
Me.rbBlink.Size = New System.Drawing.Size(52, 20)
Me.rbBlink.Text = "Blink"
'
'Form1
'
Me.ClientSize = New System.Drawing.Size(234, 270)
Me.Controls.Add(Me.Panel1)
Me.Controls.Add(Me.Label2)
Me.Controls.Add(Me.Label1)
Me.Controls.Add(Me.rbLED)
Me.Text = "PPC LED Demo"

End Sub

#End Region

Public LED As New LED

Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs)
Me.Text = LED.GetLedCount()
End Sub

Private Sub Button2_Click(ByVal sender As System.Object, ByVal e As System.EventArgs)
Dim wLed As Integer = CInt(InputBox("Led", , "0"))
Dim wStatus As Integer = CInt(InputBox("Status", , "1"))
LED.SetLedStatus(wLed, wStatus)
End Sub

Private Sub rbOff_CheckedChanged(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles rbOff.CheckedChanged
LED.SetLedStatus(0, LED.Status.OFF)
End Sub

Private Sub rbOn_CheckedChanged(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles rbOn.CheckedChanged
LED.SetLedStatus(0, LED.Status.ON)
End Sub

Private Sub rbBlink_CheckedChanged(ByVal sender As Object, ByVal e As System.EventArgs) Handles rbBlink.CheckedChanged
LED.SetLedStatus(0, LED.Status.BLINK)
End Sub

Private Sub Form1_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load

End Sub
End Class
```
