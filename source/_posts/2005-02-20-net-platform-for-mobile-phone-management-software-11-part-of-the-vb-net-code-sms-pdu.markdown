---
author: hesicong
comments: true
date: 2005-02-20 15:50:54+00:00
layout: post
slug: net-platform-for-mobile-phone-management-software-11-part-of-the-vb-net-code-sms-pdu
title: .net平台手机管理软件开发(11)—— 短信部分 VB.NET编码PDU
wordpress_id: 1216
categories:
- 其他
tags:
- .net
- pdu
---

**(十一) ****短信部分——VB.NET****编码PDU**
PDU的编码器的工作原理是解码器的逆过程。根据需要编码器只需要编码发送的PDU代码，工作相对简单。本文讲解编码思路，具体代码请参考Blog中PDUEncoder部分

我把PDU的编码分为两部分，SMS和EMS。EMS部分我只提供了ConcatenatedShortMessage的编码器。这是超长短信的编码，用得最多。
**SMS****编码**

编码一个SMS一般需要如下的信息：
TP_Data_Coding_Scheme TP_UD编码方式
TP_Destination_Address 对方号码
TP_Message_Reference 参考号码
TP_Status_Report_Request 状态报告
TP_User_Data 用户信息
TP_Validity_Priod 有效期
ServiceCenterNumber 短信中心号码

所以在编码器中存在以上的属性，并在Set中加入了处理代码，将可读信息转换成对应的十六进制信息。

特别注意的是TP_User_Data属性，它可以根据用户数据编码自动设置TP_UDL。对于纯英文编码，TP_UDL为所有的字符数；对于Unicode编码，由于一个字符由两个字节表示，TP_UDL为所有的字符数*2。注意检查TP_User_Data的长度，对于SMS来说编码后的TP_UD长度不能超过140字节。也就是说英文160个字符(140/7*8)，中文70个字符。

对于TP_UD的编码在解码器中也有说明，在此不再赘述。

我还设计了几个枚举变量：
ENUM_TP_DCS 编码方式
ENUM_TP_SRI 状态报告
ENUM_TP_VALID_PERIOD 有效期
ENUM_TP_VPF 有效期格式

这些枚举变量可以简化输入，也利于日后扩充。

当以上内容设置好以后，基本上一个短信的架子就出来了。此时调用GetSMSPDUCode进行组合，简单的把十六进制拼接起来就形成了一个完整的PDU代码。
**EMS****——ConcatenatedShortMessage****部分**

编码EMS较SMS复杂，但每条EMS的基础还是SMS，所以我直接继承了SMS类。区别主要是要处理好TP_UD和IE。对于ConcatenatedShortMessage，由于其IE和TP_UDHL占据了TP_UD的部分空间，所以每条短信英文只能容纳133字符，中文66字符。我们可以通过此信息得到短信条数。


如果TP_DCS为Unicode编码，则短信条目为：
TotalMessages = (TP_UD.Length / 4)  66 + ((TP_UD.Length / 4 Mod 66) = 0)+1

如果为7bit，则为：
TotalMessages = (tp_ud.Length  266) - ((tp_ud.Length Mod 266) = 0)+1

注意在程序中我为了简化以后的数组操作，就没有加一。

确定了短信条数以后通过一个循环就可以提取出每条短信的TP_UD。
Select Case tp_dcs
Case ENUM_TP_DCS.UCS2
tmpTP_UD = Mid(TP_UD, i * 66 * 4 + 1, 66 * 4)'When TP_UDL is odd, the max length of an Unicode string in PDU code is 66 Charactor.See [3GPP TS 23.040 V6.5.0 (2004-09] 9.2.3.24.1
Case ENUM_TP_DCS.DefaultAlphabet
tmpTP_UD = Mid(tp_ud, i * 133 * 2 + 1, 133 * 2)
End Select

此后还需要编码IE部分，关键代码是确定TP_UDL的值。对于TP_DCS为7bit来说确定此值显得比较复杂，弄不好容易出现多一个少一个的错误。
If tp_dcs = ENUM_TP_DCS.UCS2 Then
TP_UDL = tmpTP_UD.Length / 2 + 6 + 1 '6: length of IE
End If
If tp_dcs = ENUM_TP_DCS.DefaultAlphabet Then
TP_UDL = Fix((tmpTP_UD.Length + 7 * 2) * 4 / 7) '6:length of IE
End If

然后根据3GPP里关于EMS的结构的说明就可以编写出EMS PDU的处理程序。详见原代码。

如果需要扩展EMS以适应更多种类的EMS，可以参考3GPP写出更为强大的编码程序。但最关键的还是需要处理好IE以及TP_UDL。
