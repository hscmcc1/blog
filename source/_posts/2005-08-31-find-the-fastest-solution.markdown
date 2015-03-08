---
author: hesicong
comments: true
date: 2005-08-31 09:51:42+00:00
layout: post
slug: find-the-fastest-solution
title: 寻求最快解决方案
wordpress_id: 1382
categories:
- 其他
tags:
- .net
---


有两个Ulong的数L1和L2，需要将L1的第n位(bit)设置成L2的第m位(bit)。

比如L1为(binary)10101000010.....(共64位,Low ->High)，L2为(binary)0101110001....(共64位Low ->High)，设置L1的第3位(现在是1)为L2的第1位(现在是0)。

我写了一个算法，但是发现速度不快(用在加密算法里面)，大部分时间消耗在判断上了。不知道各位能不能写出速度更快的代码，或者有其他优化技巧。以下是我写的算法，大家看看……期待着大家的回答。谢谢！

```
Private Const ULong_1 As ULong = CType(1, ULong) '全部是1的ULong
Public Sub SetBit()Sub SetBit(ByRef L1 as ULong,ByVal n As Integer, ByVal L2 As ULong, ByVal m As Integer)
 If (L2 And (ULong_1 << m)) = 0 Then
 L1 = L1 And Not (ULong_1 << n)
 Else
 L1 = L1 Or (ULong_1 << n)
 End If
End Sub
```

经过苦思冥想，问题解决：

```
 Sub SetBit()Sub SetBit(ByRef L1 As ULong, ByVal n As Integer, ByRef L2 As ULong, ByVal m As Integer)
 L2 = (L2 >> m) And CULng(&H1)
 L1 = L1 And Not (CULng(&H1 << n))
 L1 = L1 + (L2 << n)
 End Sub
```

现在执行1000000次也只需要31.25ms，速度已经达到要求了。

思路如下：

思 路为把L2中的m那一位移到最右边，然后和1取AND，这样就把m这位取出来了。对于L1，我把n这一位设置为0，也就是AND 111111..0...111这样，111111..0...111来自1<<n然后取反。最后把L1中取得的那一位和L2 AND以后的值相加即可。
