---
author: hesicong
comments: true
date: 2005-07-31 16:11:30+00:00
layout: post
slug: modify-the-windows-serial-number-of-the-script
title: 修改Windows序列号的脚本
wordpress_id: 1250
categories:
- 其他
---


修改Windows序列号的脚本

```
'
' WMI Script - ChangeVLKey.vbs
'
' This script changes the product key on the computer (XP SP1 SP2 2003)
'
' Made by zyling.
'***************************************************************************
do
ON ERROR RESUME NEXT

Dim VOL_PROD_KEY
if Wscript.arguments.count<1 then
VOL_PROD_KEY=InputBox(" 使用说明(OEM版无效)："&vbCr&vbCr&" 本脚本程序将修改当前 Windows 的序列号。请先使用算号器算出匹配当前 Windows 的序列号，复制并粘贴到下面空格中。"&vbCr&vbCr&"输入序列号(默认为 XP VLK)：","Windows XP/2003 序列号更换工具","PJVCV-XCPPF-6GTKK-QXRPY-FBKGJ")
if VOL_PROD_KEY="" then
Wscript.quit
end if
else
VOL_PROD_KEY = Wscript.arguments.Item(0)
end if

VOL_PROD_KEY = Replace(VOL_PROD_KEY,"-","") 'remove hyphens if any

for each Obj in GetObject("winmgmts:{impersonationLevel=impersonate}").InstancesOf ("win32_WindowsProductActivation")

result = Obj.SetProductKey (VOL_PROD_KEY)

if err = 0 then
Wscript.echo "您的 Windows CD-KEY 修改成功。请检查系统属性。"
end if

if err <> 0 then
Wscript.echo "修改失败！请检查输入的 CD-KEY 是否与当前 Windows 版本相匹配。"
Err.Clear
end if

next
loop
```