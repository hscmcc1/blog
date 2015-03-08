---
author: hesicong
comments: true
date: 2004-12-02 15:23:16+00:00
layout: post
slug: foreign-friends-sent-me-ascii_7to8-and-decode7bit-function-concise-worth-learning
title: 外国朋友寄给我的Ascii_7to8和Decode7Bit函数，简洁明了值得学习！
wordpress_id: 1184
categories:
- 其他
tags:
- .net
- pdu
---

外国朋友寄给我的Ascii_7to8和Decode7Bit函数，简洁明了值得学习！

```
Shared Function Decode7Bit(ByVal str7BitCode As String) As String
Dim Inv7BitCode As String = InvertHexString(str7BitCode)
Dim Binary As String
Dim Result As String
Dim i As Integer
For i = 0 To Inv7BitCode.Length - 1 Step 2
Binary += ByteToBinary(CByte(Val("&H" & Inv7BitCode.Substring(i, 2))))
Next
Dim Temp As Integer
For i = 1 To Binary.Length  7
Temp = BinaryToInt(Binary.Substring(Binary.Length - i * 7, 7))
If Temp = 0 Then Temp = 64
Result = Result + ASCII_7to8(Temp) 'OLD >>>> 'Result = Result + ChrW(Temp)
Next
Return (Result)
End Function

#Region " ASCII_7to8 "
Shared ASCII_7to8() As String = { _
"@", _
"£", _
"$", _
"¥", _
"è", _
"é", _
"ù", _
"ì", _
"ò", _
"Ç", _
vbLf, _
"Ø", _
"ø", _
vbCr, _
"Å", _
"å", _
"Δ", _
"_", _
"Φ", _
"Γ", _
"Λ", _
"Ω", _
"Π", _
"Ψ", _
"Σ", _
"Θ", _
"Ξ", _
"1", _
"Æ", _
"æ", _
"ß", _
"É", _
" ", _
"!", _
Chr(34), _
"#", _
"¤", _
"%", _
"&", _
"'", _
"(", _
")", _
"*", _
"+", _
",", _
"-", _
".", _
"/", _
"0", _
"1", _
"2", _
"3", _
"4", _
"5", _
"6", _
"7", _
"8", _
"9", _
":", _
";", _
"<", _
"=", _
">", _
"?", _
"¡", _
"A", _
"B", _
"C", _
"D", _
"E", _
"F", _
"G", _
"H", _
"I", _
"J", _
"K", _
"L", _
"M", _
"N", _
"O", _
"P", _
"Q", _
"R", _
"S", _
"T", _
"U", _
"V", _
"W", _
"X", _
"Y", _
"Z", _
"Ä", _
"Ö", _
"Ñ", _
"Ü", _
"§", _
"¿", _
"a", _
"b", _
"c", _
"d", _
"e", _
"f", _
"g", _
"h", _
"i", _
"j", _
"k", _
"l", _
"m", _
"n", _
"o", _
"p", _
"q", _
"r", _
"s", _
"t", _
"u", _
"v", _
"w", _
"x", _
"y", _
"z", _
"ä", _
"ö", _
"ñ", _
"ü", _
"à" _
}
#End Region
```
