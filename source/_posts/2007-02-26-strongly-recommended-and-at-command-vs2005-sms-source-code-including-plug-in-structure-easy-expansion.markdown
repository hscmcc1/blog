---
author: hesicong
comments: false
date: 2007-02-26 03:16:42+00:00
layout: post
slug: strongly-recommended-and-at-command-vs2005-sms-source-code-including-plug-in-structure-easy-expansion
title: '[强烈推荐]手机短信和AT指令VS2005源代码，含插件式结构，方便扩展'
wordpress_id: 140
categories:
- 其他
tags:
- AT指令
- SMS
- 手机
- 短信
---

这是我2年前的一些代码，由于一个朋友需要，所以我就整理出来了，去掉了一些东西，精简为只有串口通讯、AT指令操作、短信、通讯薄的几个部分。
[download id="15"]
YOU CAN ALSO ACCESS TO:
[CodePlex OpenSource](http://www.codeplex.com/ATSMSLibrary)
or This should be a better version:
[Better Version Provided by Jianghanxia](http://www.jianghanxia.com/Blog/article.asp?id=70)
下面是说明：

//////////////////////////////////////////////////////////////////////////////
//              .NET 2.0 AT and SMS Library with PlugIn Support             //
//              Author: hesicong      Homepage: www.hesicong.net            //
//              Contact me: hesicong2005(at)163.com                         //
//////////////////////////////////////////////////////////////////////////////

Description:
This is a .NET 2.0 AT library and SMS/EMS library with Plug-In support.
You can develop your own Plug-In to support multi phones.
ATCommandBase is a class to execute AT commands.
CommPhone is a class with minimal set of functions of a phone.
Usually be used in phone detect.
PhoneControllerSDK is a SDK for you to develop your own phone plugin.
PluginForNokia is a DEMO PLUGIN for demostrate how to use these classes.
ShortMessageService is a class for decoding and encoding SMS.
Test AT is a test project offering some Test Cases to
test AT,SMS Read, Write, send.
Be sure Test Case 3 and Test Case 4 for SMS Receving and Sending should be passed!
CodeDemoForCSharp is a C# project for some C# developer.

Licence:
Be free to use use my code, and please let me know if bug was found.
And if you correct it, please give a copy of your code.
Commerial use is unaviable without my permission.
But if you use my library and create something, please give me a copy!

And ultimately, thank you to use my code, thank you to visit my homepage!
