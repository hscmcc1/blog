---
author: hesicong
comments: true
date: 2005-07-22 16:07:36+00:00
layout: post
slug: at-related-regular-expressions
title: AT相关正则表达式
wordpress_id: 1244
categories:
- 其他
---

. 取得AT指令及参数
AT+(?<CMD>w*)=(?:"?(?<PARA>w+)"?,?)*

. 取得返回的包含+CMD类型的
+(?<CMD>w*):s*(?:"?(?<PARA>w*),|(?<PARA>w+)"?)*

. AT+CMGL返回
^+CMGL:s*(?<index>d*),(?<stat>d*),(?<alpha>d*),(?<length>d*)rns*(?<PDU>w*)
