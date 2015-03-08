---
author: hesicong
comments: true
date: 2005-02-19 15:48:48+00:00
layout: post
slug: net-platform-for-mobile-phone-management-software-10-decode-pdu-sms-part-of-the-vb-net
title: .net平台手机管理软件开发(10)—— 短信部分 VB.NET解码PDU
wordpress_id: 1214
categories:
- 其他
tags:
- .net
- pdu
---

**(十) ****短信部分——VB.NET****解码PDU**

早在2004年1月份我就开始初步的研究PDU的编码解码原理，对于PDU也有比较深刻的认识。随后按照3GPP协议写了一个PDU Decoder，后来写成PDU Decoder文章发表在CodeProject上面，有几个好心的外国网友给我指出了一些BUG，现在成了一个比较完善的Decoder。本文讲解编码器的构成以及我所使用的解码方法及技巧。
**解码器的构成**
NameSpace **SMS**
** Decoder**
MustInheritClass** SMSBase**
Class ** EMS_RECEIVED**
Class **EMS_SUBMIT**
Class **SMS_RECEIVED**
Class **SMS_STATUS_REPORT**
Class **SMS_SUBMIT**
Class **PDUDecoder**
**SMSBase****部分**
SMSBase类是必须继承类，它包含了PDU的基本结构以及一些相关辅助函数，是最基本的类，其他的类都是从SMSBase继承的。通过SMSBase的Shared函数GetSMSType可以得到PDU的类型，从而确定使用的Class。
SMSBase包含了所有短信类型所共有的基本信息部分以及一个指示短信类型的枚举SMSType，继承的类扩展其特有的基本信息部分。
Public SCAddressLength As Byte 'Service Center Address length
Public SCAddressType As Byte 'Service Center Type[See GSM 03.40]
Public SCAddressValue As String 'Service Center nuber
Public FirstOctet As Byte 'See GSM 03.40

Public TP_PID As Byte
Public TP_DCS As Byte
Public TP_UDL As Byte
Public TP_UD As String
Public Text As String
Public Type As SMSType
Public UserData As String

Public Enum SMSType
SMS_RECEIVED = 0
SMS_STATUS_REPORT = 2
SMS_SUBMIT = 1
EMS_RECEIVED = 64 'It is "Reserved" on my phone??
EMS_SUBMIT = 65
End Enum

SMSBase中定义了一个必须重写的过程GetOrignalData，其参数为PDUCode，目的是为了得到PDU的基本信息。不同的短信类型具有不同的解码过程，所以作为一个必须重写的函数。
Public MustOverride Sub GetOrignalData(ByVal PDUCode As String)

SMSBase中还有一系列的辅助函数，具体实现方法见源代码：

处理PDU代码的：

处理PDU代码我运用了自称为“按需裁减”的技巧，就是把需要的数据提取出来解码，然后从原PDUCode中删除这一部分，在传递给下一个函数处理。这样就不用考略具体的偏移量，简化了操作，增强了适应性。为了能够减少返回处理过的PDUCode麻烦，我使用了ByRef，执行过程以后PDUCode就自动被裁减了。
'Get a byte from PDU string
Shared Function GetByte(ByRef PDUCode As String) As Byte
'Get a string of certain length
Shared Function GetString(ByRef PDUCode As String, ByVal Length As Integer) As String
'Get date from SCTS format
Shared Function GetDate(ByRef SCTS As String) As Date
'Swap two bit
Shared Function Swap(ByRef TwoBitStr As String) As String
'Get phone address
Shared Function GetAddress(ByRef Address As String) As String
Shared Function GetSMSType(ByVal PDUCode As String) As SMSBase.SMSType
TP-UD解码部分：
TP-UD的解码的任务主要集中在Unicode的解码和7BitCode的解码。其中Unicode的解码很方便，只需要将两个字节的PDUCode通过Val函数转换成为数字，在通过ChrW函数即可得到。

而7BitCode就显得比较难，下面以Test四个字符简单介绍其基本原理，具体的编码方式请参考相关资料。
Byte1 11010100 0xD4
Byte2 11110010 0xF2
Byte3 10011100 0x9C
Byte4 00001110 0x0E

注：各字符二进制代码：
T：1010100 e：1100101 s：1110011 t：1110100

从这个例子可以看出一个Byte包含了一个字符的ASCII码的二进制部分及后续字符的二进制部分的低位。这样8个字符可以压缩成为7个Byte，SMS中140Byte的TP-UD长度就可以容纳160个英文字母。

通过观察可以看出，只要我们从后到前把所有的二进制代码拼接到一块，就能够方便的处理，上面例子通过拼接后得到：
00001110100111001111001011010100

我们可以直接通过从后往前的按7个一组的原则进行截取在处理就可以得到解码后的代码。为了编程的方便，我设计了一个简单易懂的解码过程，比起通过做乘除法来进行运算的简单，但最终效率不及它。但我想在普通场合应用也绰绰有余了。
.  Decode7Bit得到一个PDU的TP-UD部分
.  InvertHexString反转十六进制代码：例如123456=〉563412
.  Binary字符串得到反转后的十六进制代码的二进制表示。注意这里依然使用字符串来表示二进制，为了便于“拼接”和“切割”
.  根据charCount所提供的字符数(来自TP_UDL)按7个一组从字符串位往前截取，并用Chr函数转换成ASCII码。

以下是一些函数的声明部分，具体函数请参见Blog内的PDUDecoder
'Deoce a unicode string
Shared Function DecodeUnicode(ByVal strUnicode As String) As String
'Decode 7bit to English
Shared Function InvertHexString(ByVal HexString As String) As String
Shared Function ByteToBinary(ByVal Dec As Byte) As String
Shared Function BinaryToInt(ByVal Binary As String) As Integer
Shared Function Decode7Bit(ByVal str7BitCode As String, ByVal charCount As Integer) As String

**SMS_SUBMIT****、SMS_RECEIVED****、SMS_STATUS_REPORT**

由于SMS_RECEIVED、SMS_STATUS_REPORT与SMS_SUBMIT比较相似，所以我重点讲讲SMS_SUBMIT。

当用SMSBase的GetSMSType确定一个PDUCode为SMS_SUBMIT时，就可以声明一个SMS_SUBMIT类的实例，通过传递此PDUCode作为构造函数的参数。构造函数立即调用GetOrignalData函数解码。

参考协议知道SMS_SUBMIT比SMSBase多出以下部分：
Public TP_MR As Byte
Public DesAddressLength As Byte
Public DesAddressType As Byte
Public DesAddressValue As String
Public TP_VP As Byte

参考协议我们可以很方便的得到GetOrignalData函数的实现：
Public Overrides Sub GetOrignalData(ByVal PDUCode As String)
SCAddressLength = GetByte(PDUCode)
SCAddressType = GetByte(PDUCode)
SCAddressValue = GetAddress((GetString(PDUCode, (SCAddressLength - 1) * 2)))
FirstOctet = GetByte(PDUCode)

TP_MR = GetByte(PDUCode)

DesAddressLength = GetByte(PDUCode)
DesAddressType = GetByte(PDUCode)
DesAddressLength += DesAddressLength Mod 2
DesAddressValue = GetAddress((GetString(PDUCode, DesAddressLength)))

TP_PID = GetByte(PDUCode)
TP_DCS = GetByte(PDUCode)
TP_VP = GetByte(PDUCode)
TP_UDL = GetByte(PDUCode)
TP_UD = GetString(PDUCode, TP_UDL * 2)
End Sub

这就完成了整个解码过程，通过SMSBase的巧妙设计，此解码过程显得简单方便。

**EMS_SUBMIT****、EMS_RECEIVED**

对于EMS(增强型短信)，其基本结构和SMS类似，主要的区别就是Information Element(IE)。所以EMS_SUBMIT继承了SMS_SUBMIT，EMS_RECEIVED继承了SMS_RECEIVED

参考3GPP协议EMS部分我们可以做出以下的结构和定义
Public Structure InfoElem 'See document "How to create EMS"
Public Identifier As Byte
Public Length As Byte
Public Data As String
End Structure
Public TP_UDHL As Byte

为了得到IE我写了一个函数：
Shared Function GetIE(ByVal IECode As String) As InfoElem()
Dim tmp As String = IECode, t As Integer = 0
Dim result() As InfoElem
Do Until IECode = ""
ReDim Preserve result(t)
With result(t)
.Identifier = GetByte(IECode)
.Length = GetByte(IECode)
.Data = GetString(IECode, .Length * 2)
End With
t += 1
Loop
Return result
End Function

然后参考协议可以写出GetOrignalData函数。具体就不再赘述。

**PDUDecoder**

这个类的由一个结构，一个重要的解码函数，组成。

结构定义了需要取得的基本信息，可以视需要修改。我这里提供一个范例
Public Structure BaseInfo
Public SourceNumber As String
Public DestinationNumber As String
Public ReceivedDate As Date
Public Text As String
Public Type As SMS.Decoder.SMSBase.SMSType
Public EMSTotolPiece As Integer
Public EMSCurrentPiece As Integer
Public StatusFromReport As SMS_STATUS_REPORT.EnumStatus
Public DestinationReceivedDate
End Structure

解码函数的声明如下：
Public Shared Function Decode(ByVal PDUCode As String) As BaseInfo

内部主要处理步骤如下(源代码请参考PDUDecoder)
1. 根据SMSBase的GetSMSType函数得到短信类型SMSType
2. 根据SMSType生成对应的类的实例
3. 解码PDU，得到基本结构
4. 通过基本结构得到BaseInfo结构里面需要的数据
5. 通过decode7bit或者decodeUnicode函数得到TP_UD数据


到此为止，这就是整个PDU Decoder的详细介绍，具体使用可以参见Siemens Support Tool里面相关部分，在此不再赘述。
