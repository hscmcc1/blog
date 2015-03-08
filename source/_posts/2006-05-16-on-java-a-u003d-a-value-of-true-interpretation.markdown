---
author: hesicong
comments: true
date: 2006-05-16 10:07:40+00:00
layout: post
slug: on-java-a-u003d-a-value-of-true-interpretation
title: 关于在Java中 a!=a 值为真的解释
wordpress_id: 1426
categories:
- 其他
tags:
- .net
- java
---

In java spec , the primitive type is implemented in a certain defined way. The floating-types are implemented in IEEE-754 standard.
Ref from java spec:
The IEEE 754 standard includes not only positive and negative sign-magnitude numbers, but also positive and negative zeros, positive and negative _infinities_, and a special _Not-a-Number _value (hereafter abbreviated as "NaN"). The NaN value is used to represent the result of certain invalid operations such as dividing zero by zero.
So, in this case , java specified that a!=a is true if the variable is NaN. So does the case i present yesterday (a<b)==(a>=b) if either a or b is NaN.
Now I wil give you a example show it:
.........
double a=0;
double b=0;
a=a/b;
if(a!=a) System.out.println("OK, a!=a is true!");
if((a<b)==(a>=b)) System.out.println("........");

But I think the problem is a little tricky, If you have not yet read something from the java spec or something other, you will not  know that. But when the problem been proposed to us ,we should think about the problem in a logical way ,but not the way coding it to see if a=0, and then  a!=a is false. What i want to see is not the result of this ,but the thinking way of the job seeker.

.net 2.0里面也是这样

Module Test
Sub Main()
Dim a as double=0
Dim b as double=0
a=a/b
console.writeline("a ={0}",a)
console.writeline("a<>a ={0}",a<>a)
console.writeline("a=a ={0}",a=a)
console.writeline("a<b ={0}",a<b)
console.writeline("a>b ={0}",a>b)
console.writeline("a=b ={0}",a=b)
End Sub
end module

结果：
a =非数字
a<>a =True
a=a =False
a<b =False
a>b =False
a=b =False
