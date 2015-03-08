---
author: hesicong
comments: true
date: 2009-01-16 14:39:42+00:00
layout: post
slug: python-integer-is-converted-to-any-band-u0026ltu003d-36
title: python:整数转换为任意进制(
wordpress_id: 1536
categories:
- 其他
tags:
- python
---

javascript提供了一个number.toString(baseNum)的函数，能够将number转换为36进制以下的字符串。
python里面仅提供了将字符串转换为整数的函数，并没有提供相应的函数将整数转换为任意进制的字符串的函数(如果有的话，请告诉我一声)。[在网上找到了答案](http://code.activestate.com/recipes/65212/)：

``` python
def base10toN(num,n):
    """Change a  to a base-n number.
    Up to base-36 is supported without special notation."""
    num_rep={10:'a',
         11:'b',
         12:'c',
         13:'d',
         14:'e',
         15:'f',
         16:'g',
         17:'h',
         18:'i',
         19:'j',
         20:'k',
         21:'l',
         22:'m',
         23:'n',
         24:'o',
         25:'p',
         26:'q',
         27:'r',
         28:'s',
         29:'t',
         30:'u',
         31:'v',
         32:'w',
         33:'x',
         34:'y',
         35:'z'}
    new_num_string=''
    current=num
    while current!=0:
        remainder=current%n
        if 36>remainder>9:
            remainder_string=num_rep[remainder]
        elif remainder>=36:
            remainder_string='('+str(remainder)+')'
        else:
            remainder_string=str(remainder)
        new_num_string=remainder_string+new_num_string
        current=current/n
    return new_num_string
```
    还有一个简化版：

``` python
def baseN(num,b):
  return ((num == 0) and  "0" ) or ( baseN(num // b, b).lstrip("0") + "0123456789abcdefghijklmnopqrstuvwxyz"[num % b])
```