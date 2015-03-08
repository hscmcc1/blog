---
author: hesicong
comments: true
date: 2005-03-17 15:56:36+00:00
layout: post
slug: pda-battery-info-vb-net-net-cf-source-code
title: PDA Battery Info (VB.Net+.Net CF) Source Code
wordpress_id: 1227
categories:
- 其他
tags:
- .net
- pda
---

PDA Battery Info (VB.Net+.Net CF) Source Code

```
'======================================
' Get PDA Battery Info
'Summary:
' Using P/Invoke to get Battery Info.
'This technique is introduced in MSDN
'Library - January 2005 and this program
'is written based on the sample program
'of ms-help://MS.MSDNQTR.2005JAN.1033/dncfhowto/html/getpowstat.htm
'Functions:
' This program get PDA's battery info-
'mation containing main battery lifetime,
'voltage,current,and backup battery info.
'
'2005/3/17
'Hesicong
'http://dream-world.nease.net
'http://blog.csdn.net/hesicong
'mailto:hesicong@mail.sc.cninfo.net
Imports System
Imports System.Drawing
Imports System.Collections
Imports System.Windows.Forms
Imports System.Data
Imports System.Runtime.InteropServices
' Summary description for Form1.
Public Class BatteryInfo
Inherits System.Windows.Forms.Form
Friend WithEvents TextBox1 As System.Windows.Forms.TextBox
Private mainMenu1 As System.Windows.Forms.MainMenu
Public Sub New()
'
' Required for Windows Form Designer support
'
InitializeComponent()
End Sub 'New
'
' TODO: Add any constructor code after InitializeComponent call
'
' Clean up any resources being used.
Protected Overloads Overrides Sub Dispose(ByVal disposing As Boolean)
MyBase.Dispose(disposing)
End Sub 'Dispose
#Region "Windows Form Designer generated code"
' Required method for Designer support - do not modify
' the contents of this method with the code editor.
Friend WithEvents MenuItem4 As System.Windows.Forms.MenuItem
Friend WithEvents MenuItem5 As System.Windows.Forms.MenuItem
Friend WithEvents MenuItem7 As System.Windows.Forms.MenuItem
Friend WithEvents Timer1 As System.Windows.Forms.Timer
Friend WithEvents mnuAutoRefresh As System.Windows.Forms.MenuItem
Friend WithEvents mnuAbout As System.Windows.Forms.MenuItem
Friend WithEvents MenuItem1 As System.Windows.Forms.MenuItem
Private Sub InitializeComponent()
Me.mainMenu1 = New System.Windows.Forms.MainMenu
Me.MenuItem4 = New System.Windows.Forms.MenuItem
Me.MenuItem5 = New System.Windows.Forms.MenuItem
Me.mnuAutoRefresh = New System.Windows.Forms.MenuItem
Me.MenuItem7 = New System.Windows.Forms.MenuItem
Me.mnuAbout = New System.Windows.Forms.MenuItem
Me.TextBox1 = New System.Windows.Forms.TextBox
Me.Timer1 = New System.Windows.Forms.Timer
Me.MenuItem1 = New System.Windows.Forms.MenuItem
'
'mainMenu1
'
Me.mainMenu1.MenuItems.Add(Me.MenuItem4)
Me.mainMenu1.MenuItems.Add(Me.mnuAbout)
'
'MenuItem4
'
Me.MenuItem4.MenuItems.Add(Me.MenuItem5)
Me.MenuItem4.MenuItems.Add(Me.mnuAutoRefresh)
Me.MenuItem4.MenuItems.Add(Me.MenuItem1)
Me.MenuItem4.MenuItems.Add(Me.MenuItem7)
Me.MenuItem4.Text = "Menu"
'
'MenuItem5
'
Me.MenuItem5.Text = "Refresh"
'
'mnuAutoRefresh
'
Me.mnuAutoRefresh.Text = "AutoRefresh"
'
'MenuItem7
'
Me.MenuItem7.Text = "End"
'
'mnuAbout
'
Me.mnuAbout.Text = "About"
'
'TextBox1
'
Me.TextBox1.Location = New System.Drawing.Point(8, 8)
Me.TextBox1.Multiline = True
Me.TextBox1.ScrollBars = System.Windows.Forms.ScrollBars.Horizontal
Me.TextBox1.Size = New System.Drawing.Size(224, 256)
Me.TextBox1.Text = ""
'
'Timer1
'
Me.Timer1.Interval = 1000
'
'MenuItem1
'
Me.MenuItem1.Text = "SetRefreshInterval"
'
'BatteryInfo
'
Me.Controls.Add(Me.TextBox1)
Me.Menu = Me.mainMenu1
Me.Text = "BatteryInfo"
End Sub 'InitializeComponent
#End Region
' The main entry point for the application.
Shared Sub Main()
Application.Run(New BatteryInfo)
End Sub 'Main
Public Class SYSTEM_POWER_STATUS_EX2
Public ACLineStatus As Byte
Public BatteryFlag As Byte
Public BatteryLifePercent As Byte
Public Reserved1 As Byte
Public BatteryLifeTime As System.UInt32
Public BatteryFullLifeTime As System.UInt32
Public Reserved2 As Byte
Public BackupBatteryFlag As Byte
Public BackupBatteryLifePercent As Byte
Public Reserved3 As Byte
Public BackupBatteryLifeTime As System.UInt32
Public BackupBatteryFullLifeTime As System.UInt32
Public BatteryVoltage As System.UInt32
Public BatteryCurrent As System.UInt32
Public BatteryAverageCurrent As System.UInt32
Public BatteryAverageInterval As System.UInt32
Public BatterymAHourConsumed As System.UInt32
Public BatteryTemperature As System.UInt32
Public BackupBatteryVoltage As System.UInt32
Public BatteryChemistry As Byte
End Class 'SYSTEM_POWER_STATUS_EX2

Public Class SYSTEM_POWER_STATUS_EX
Public ACLineStatus As Byte
Public BatteryFlag As Byte
Public BatteryLifePercent As Byte
Public Reserved1 As Byte
Public BatteryLifeTime As System.UInt32
Public BatteryFullLifeTime As System.UInt32
Public Reserved2 As Byte
Public BackupBatteryFlag As Byte
Public BackupBatteryLifePercent As Byte
Public Reserved3 As Byte
Public BackupBatteryLifeTime As System.UInt32
Public BackupBatteryFullLifeTime As System.UInt32
End Class 'SYSTEM_POWER_STATUS_EX
<DllImport("coredll")> _
Private Shared Function GetSystemPowerStatusEx(ByVal lpSystemPowerStatus As SYSTEM_POWER_STATUS_EX, ByVal fUpdate As Boolean) As System.UInt32
End Function
<DllImport("coredll")> _
Private Shared Function GetSystemPowerStatusEx2(ByVal lpSystemPowerStatus As SYSTEM_POWER_STATUS_EX2, ByVal dwLen As System.UInt32, ByVal fUpdate As Boolean) As System.UInt32
End Function
Private Sub Form1_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles MyBase.Load
RefreshStatus()
End Sub 'Form1_Load
Private Sub RefreshStatus()
Dim status As New SYSTEM_POWER_STATUS_EX
Dim status2 As New SYSTEM_POWER_STATUS_EX2
TextBox1.Text = ""
TextBox1.Text = "Sample Time:" & Format(Now, "HH:mm:ss") + vbCrLf + vbCrLf
With TextBox1
If Convert.ToInt32(GetSystemPowerStatusEx(status, False)) = 1 Then
'Do some further work here
End If
If Convert.ToInt32(GetSystemPowerStatusEx2(status2, Convert.ToUInt32(Marshal.SizeOf(status2)), False)) = Marshal.SizeOf(status2) Then
Select Case CInt(status2.ACLineStatus.ToString)
Case 0
.Text += "AC Status: Offline" + vbCrLf
Case 1
.Text += "AC Status: Online" + vbCrLf
Case Else
.Text += "AC Status: Unknown" + vbCrLf
End Select
.Text += vbCrLf
.Text += "MainBattery" + vbCrLf
.Text += "-LifePercent:" & status2.BatteryLifePercent.ToString + vbCrLf
.Text += "-Voltage:" & (CInt(status2.BatteryVoltage.ToString) / 1000).ToString + "V" + vbCrLf
.Text += "-Current:" & status2.BatteryCurrent.ToString + "mA" + vbCrLf
.Text += "-Temperature:" & (CInt(status2.BatteryTemperature.ToString) / 10).ToString + "℃" + vbCrLf
.Text += vbCrLf
.Text += "BackupBattery" + vbCrLf
.Text += "-LifePercent:" & status2.BackupBatteryLifePercent.ToString + vbCrLf
.Text += "-Voltage:" & (CInt(status2.BackupBatteryVoltage.ToString) / 1000).ToString + "V" + vbCrLf
End If
End With
End Sub
Private Sub MenuItem3_Click(ByVal sender As System.Object, ByVal e As System.EventArgs)
Application.Exit()
End Sub
Private Sub MenuItem1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs)
RefreshStatus()
End Sub
Private Sub MenuItem7_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MenuItem7.Click
Application.Exit()
End Sub
Private Sub MenuItem5_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MenuItem5.Click
RefreshStatus()
End Sub
Private Sub mnuAutoRefresh_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles mnuAutoRefresh.Click
mnuAutoRefresh.Checked = Not (mnuAutoRefresh.Checked)
Timer1.Enabled = mnuAutoRefresh.Checked
End Sub
Private Sub Timer1_Tick(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Timer1.Tick
RefreshStatus()
End Sub
Private Sub mnuAbout_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles mnuAbout.Click
MsgBox("Program by Hesicong" & vbCrLf & "http://dream-world.nease.net" & vbCrLf & " http://blog.csdn.net/hesicong" & vbCrLf & "QQ:38288890", MsgBoxStyle.OKOnly, "About")
End Sub
Private Sub MenuItem1_Click_1(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MenuItem1.Click
Timer1.Interval = CInt(InputBox("Please enter autorefresh interval(s)", "BatteryInfo", "1")) * 1000
End Sub

End Class 'Form1
```
