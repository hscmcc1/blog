---
author: hesicong
comments: true
date: 2005-08-21 09:50:49+00:00
layout: post
slug: little-information-3gpp-document-naming
title: 小资料——3GPP文档命名规则
wordpress_id: 1380
categories:
- 其他
tags:
- Phone
---


今天正在读Mobile Messaging Technologies and Services一书，其中阐述了3GPP文档命名的原则。我把它翻译成中文并整理一下作为一个小资料供查阅。更为精确的描述请参考Mobile Messaging Technologies and Services一书。

**3GPP规范：命名方案
** 每份3GPP技术文档，技术报告(TR)或者技术规范(TR)，都被一个Reference唯一标示。这个Reference以3GPP前缀开始，后跟 两个字符表示文档的类型(TS为技术报告，TR为技术规范)。在文档类型之后紧接着是规范的号码。规范号码具有aa.bbb或者aa.bb两种形式。其中 aa指示文档的适用范围(见表1)。规范号码后面是版本Vx.y.z，其中x表示release，y表示技术版本，z表示修订版本。

另外注意每个release都有一个冻结日期，一般3GPP协议在冻结以后就不再修改。一般冻结日期为1年。

例如：以下文档定义了MMS Stage 1:
Reference 3GPP TS 23.140 V5.2.0
Title          Multimedia Messaging Service, Stage 1

文档中有Stage 1,2,3之分。Stage 1规范了从服务使用者的角度阐述的一个服务；Stage 2规范描述了对需要解决的问题的逻辑分析；Stage 3就是技术实现。
3GPP规范可以从[http://www.3gpp.org](http://www.3gpp.org/) 获得。


表一 编号
<table cellpadding="0" cellspacing="0" border="1" width="490" >
<tbody >
<tr >

<td width="98" valign="top" >Range for GSM up
</td>

<td width="100" valign="top" >Range for GSM
</td>

<td width="112" valign="top" >Range for UMTS
</td>

<td width="179" valign="top" >Type of use
</td>
</tr>
<tr >

<td width="98" >to and including
</td>

<td width="100" >release
</td>

<td width="112" >release 1999
</td>

<td width="179" valign="top" >
</td>
</tr>
<tr >

<td width="98" valign="top" >release 99
</td>

<td width="100" valign="top" >onwards
</td>

<td width="112" valign="top" >onwards
</td>

<td width="179" valign="top" >
</td>
</tr>
<tr >

<td width="98" >01.bb
</td>

<td width="100" >41.bbb
</td>

<td width="112" >21.bbb
</td>

<td width="179" >Requirement specifications
</td>
</tr>
<tr >

<td width="98" valign="top" >02.bb
</td>

<td width="100" valign="top" >42.bbb
</td>

<td width="112" valign="top" >22.bbb
</td>

<td width="179" valign="top" >Service aspects
</td>
</tr>
<tr >

<td width="98" >03.bb
</td>

<td width="100" >43.bbb
</td>

<td width="112" >23.bbb
</td>

<td width="179" >Technical realizations
</td>
</tr>
<tr >

<td width="98" >04.bb
</td>

<td width="100" >44.bbb
</td>

<td width="112" >24.bbb
</td>

<td width="179" >Signalling protocols
</td>
</tr>
<tr >

<td width="98" valign="top" >05.bb
</td>

<td width="100" valign="top" >45.bbb
</td>

<td width="112" valign="top" >25.bbb
</td>

<td width="179" valign="top" >Radio access aspects
</td>
</tr>
<tr >

<td width="98" >06.bb
</td>

<td width="100" >46.bbb
</td>

<td width="112" >26.bbb
</td>

<td width="179" >Codecs
</td>
</tr>
<tr >

<td width="98" >07.bb
</td>

<td width="100" >47.bbb
</td>

<td width="112" >27.bbb
</td>

<td width="179" >Data
</td>
</tr>
<tr >

<td width="98" >08.bb
</td>

<td width="100" >48.bbb
</td>

<td width="112" >28.bbb
</td>

<td width="179" >Signalling protocols
</td>
</tr>
<tr >

<td width="98" valign="top" >09.bb
</td>

<td width="100" valign="top" >49.bbb
</td>

<td width="112" valign="top" >29.bbb
</td>

<td width="179" valign="top" >Core network signalling protocols
</td>
</tr>
<tr >

<td width="98" >10.bb
</td>

<td width="100" >50.bbb
</td>

<td width="112" >30.bbb
</td>

<td width="179" >Programme management
</td>
</tr>
<tr >

<td width="98" valign="top" >11.bb
</td>

<td width="100" valign="top" >51.bbb
</td>

<td width="112" valign="top" >31.bbb
</td>

<td width="179" valign="top" >SIM/USIM
</td>
</tr>
<tr >

<td width="98" valign="top" >12.bb
</td>

<td width="100" valign="top" >52.bbb
</td>

<td width="112" valign="top" >32.bbb
</td>

<td width="179" valign="top" >Charging and OAM&
</td>
</tr>
<tr >

<td width="98" >13.bb
</td>

<td width="100" valign="top" >
</td>

<td width="112" valign="top" >
</td>

<td width="179" >Regulatory test specifications
</td>
</tr>
<tr >

<td width="98" valign="top" >
</td>

<td width="100" valign="top" >
</td>

<td width="112" valign="top" >33.bbb
</td>

<td width="179" valign="top" >Security aspects
</td>
</tr>
<tr >

<td width="98" valign="top" >
</td>

<td width="100" valign="top" >
</td>

<td width="112" valign="top" >34.bbb
</td>

<td width="179" valign="top" >Test specifications
</td>
</tr>
<tr >

<td width="98" valign="top" >
</td>

<td width="100" valign="top" >
</td>

<td width="112" >35.bbb
</td>

<td width="179" >Algorithms
</td>
</tr>
</tbody></table>

表二  对应release的冻结日期
<table cellpadding="0" cellspacing="0" border="1" width="487" >
<tbody >
<tr >

<td colspan="2" valign="top" width="113" >GSM/edge release
</td>

<td width="75" valign="top" >3G release
</td>

<td width="71" valign="top" >
Abbreviated

</td>

<td width="82" valign="top" >Specification
</td>

<td width="78" valign="top" >Specification
</td>

<td width="68" valign="top" >Freeze date
</td>
</tr>
<tr >

<td colspan="2" valign="top" width="113" >
</td>

<td width="75" valign="top" >
</td>

<td width="71" >
name

</td>

<td width="82" >number
</td>

<td width="78" >version
</td>

<td width="68" valign="top" >
</td>
</tr>
<tr >

<td colspan="2" valign="top" width="113" >
</td>

<td width="75" valign="top" >
</td>

<td width="71" valign="top" >
</td>

<td width="82" valign="top" >format
</td>

<td width="78" valign="top" >format
</td>

<td width="68" valign="top" >
</td>
</tr>
<tr >

<td width="54" >Phase +
</td>

<td width="60" >release
</td>

<td width="75" >
Release

</td>

<td width="71" >Rel-
</td>

<td width="82" >aaa.bb (3G)
</td>

<td width="78" >6.x.(3G)
</td>

<td width="68" >June 2003
</td>
</tr>
<tr >

<td width="54" valign="top" >Phase +
</td>

<td width="60" >release
</td>

<td width="75" >
Release

</td>

<td width="71" >Rel-
</td>

<td width="82" >aa.bb (GSM)
</td>

<td width="78" >5.x.(GSM)
</td>

<td width="68" >March 2002
</td>
</tr>
<tr >

<td width="54" valign="top" >Phase +
</td>

<td width="60" >release
</td>

<td width="75" >
Release

</td>

<td width="71" >Rel-
</td>

<td width="82" >aaa.bb (3G)
</td>

<td width="78" >4.x.(3G)
</td>

<td width="68" >March 2001
</td>
</tr>
<tr >

<td width="54" valign="top" >
</td>

<td width="60" valign="top" >
</td>

<td width="75" valign="top" >
</td>

<td width="71" valign="top" >
</td>

<td width="82" >aa.bb (GSM)
</td>

<td width="78" >9.x.(GSM)
</td>

<td width="68" valign="top" >
</td>
</tr>
<tr >

<td width="54" valign="top" >Phase +
</td>

<td width="60" >release 99
</td>

<td width="75" >
Release 99

</td>

<td width="71" >R99
</td>

<td width="82" >aaa.bb (3G)
</td>

<td width="78" >3.x.(3G)
</td>

<td width="68" >March 2000
</td>
</tr>
<tr >

<td width="54" valign="top" >
</td>

<td width="60" valign="top" >
</td>

<td width="75" valign="top" >
</td>

<td width="71" valign="top" >
</td>

<td width="82" valign="top" >aa.bb (GSM)
</td>

<td width="78" valign="top" >8.x.(GSM)
</td>

<td width="68" valign="top" >
</td>
</tr>
<tr >

<td width="54" valign="top" >Phase +
</td>

<td width="60" >release 98
</td>

<td width="75" valign="top" >
</td>

<td width="71" >R98
</td>

<td width="82" >aa.bb
</td>

<td width="78" >7.x.
</td>

<td width="68" >Early 1999
</td>
</tr>
<tr >

<td width="54" valign="top" >Phase +
</td>

<td width="60" >release 97
</td>

<td width="75" valign="top" >
</td>

<td width="71" >R97
</td>

<td width="82" >aa.bb
</td>

<td width="78" >6.x.
</td>

<td width="68" >Early 1998
</td>
</tr>
<tr >

<td width="54" valign="top" >Phase +
</td>

<td width="60" >release 96
</td>

<td width="75" valign="top" >
</td>

<td width="71" >R96
</td>

<td width="82" >aa.bb
</td>

<td width="78" >5.x.
</td>

<td width="68" >Early 1997
</td>
</tr>
<tr >

<td width="54" >Phase
</td>

<td width="60" valign="top" >
</td>

<td width="75" valign="top" >
</td>

<td width="71" >PH2
</td>

<td width="82" >aa.bb
</td>

<td width="78" >4.x.
</td>

<td width="68" >1995
</td>
</tr>
<tr >

<td width="54" >Phase
</td>

<td width="60" valign="top" >
</td>

<td width="75" valign="top" >
</td>

<td width="71" >PH1
</td>

<td width="82" >aa.bb
</td>

<td width="78" >3.x.
</td>

<td width="68" >1992
</td>
</tr>
</tbody></table>
