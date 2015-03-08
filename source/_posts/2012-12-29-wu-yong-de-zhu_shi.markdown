---
author: hesicong
comments: true
date: 2012-12-29 15:53:09+00:00
layout: post
slug: wu_yong_de_zhu_shi
title: 神一样的注释
wordpress_id: 3197
categories:
- 生活
---

曾经的我被批评过很多次写的注释写的很糟糕，我原本以为把函数名取得长一点，意思更明确一些，变量更易懂一些，就不用写注释了。结果后来我去年的代码给评奖的时候，说注释不规范，每个函数就几行，看不懂。有一次同事问我新增代码多少，我看了一下代码统计，发现不多，但是突然注意到注释率56%。也就是说100行代码，有56行注释。真是神一样的代码。
这次我倒是真的“专心”写了注释的。每一个函数头都谢了注释，即便是构造函数，也写了注释。

```
/*******************************************
* 作用：构造函数* 输入：无
*输出：无
*作者：xxxxxxxxxxx
********************************************/
XXXXXXXXX:XXXXXXXX()
{
}
```

这样，两行代码，写了6行注释，会得到很高的注释率。但，这个注释，真的有用么？
神一样的注释还有：

```
/**********************************************
Function name:*
Param:*
IN:*
OUT:*
Reference by:*
Called by:*
Return:
History:*
Author:*
Date:*
*********************************************/
```

刚进公司的时候，发的是一个17寸的LCD，一个代码要滚很多屏幕才能滚完。这些注释都是Source Insight的一个插件搞得，相当霸道。但很多注释完全是为了注释而注释，一个有用信息都没有。每次看到这种注释都有种冲动想把它干掉。前段时间新员工代码检视被软件部经理说注释不齐全，说这些要补齐。幸好当时我看到了，她正在机械的补齐Reference by这些注释。我立刻制止，给她一份我的注释

```
/******************************************
功能：
参数：
返回值：
作者：
******************************************/
```

并且，如果没有的信息，不要填。如果返回值是void，这一行就干掉。
我认为注释真的要写一些有意义的东东，写一些不容易变化的，重要的。例如什么函数名写来干啥，下面不是立刻就有？构造函数和析构，如果不是带参数的，也建议不写。谁不知道这是个构造函数？
还有一种特色的注释，如果你打开华为的G330手机的ROM的build.prop文件，你就会看到这种神奇的注释：

```
persist.fuse_sdcard=false
#DTS2012011902027 guanjunujie 20120120 begin
ro.config.hw_allowformat=true
#DTS2012011902027 guanjunujie 20120120 end
#DTS2012011902069 guanjunujie 20120120 begin
ro.config.hw_allow_ums_and_mtp=true
#DTS2012011902069 guanjunujie 20120120 end
#DTS2012021607935 guanjunujie 20120214 begin
ro.config.switchPrimaryVolume=true
#DTS2012021607935 guanjunujie 20120214 end
# DTS2012022802590 wuye00193878 20120229 begin
ro.config.internal_sdcard=yes
ro.config.hw_cdma_cdg=false
# DTS2012022802590 wuye00193878 20120229 end
#/*< DTS2012031303262 yufei 20120313 begin */
#/*< DTS2012022306777 yufei 20120301 begin */ #/*DTS2012022306777 yufei 20120301 end >*/
#/*DTS2012031303262 yufei 20120313 end >*/
# /* # /* # /* */
# /* DTS2012030302649 tiandazhang 20120303 end> */
# /* DTS2012031100199 tiandazhang 20120311 end> */
#DTS2012030300592 Cheng Wei c81003953 2012-03-09 add begin.
ro.build.characteristics=default
#DTS2012030300592 Cheng Wei c81003953 2012-03-09 add end.
#DTS2012022804910 chensaitao 20120323 begin
ro.config.hw_test_version=false
#DTS2012022804910 chensaitao 20120323 end
# DTS2012062806152 z81003396 20120628 Delete ro.huawei.cust.drm.fl_only=true.
#/**/
# DTS2012032607960 tiandazhang 20120327 begin
ro.config.widevine_level3=true
# DTS2012032607960 tiandazhang 20120327 end
#/**/

# DTS2012041703860 zhangqijia 20120417 begin
# DTS2012080800950 baitao 20120905 begin
ro.com.google.gmsversion=ics_signed_r4
# DTS2012080800950 baitao 20120905 end
# DTS2012041703860 zhangqijia 20120417 end

#DTS2012022002232 wangwenbo 20120220 begin
ro.config.hwft_simrefresh=true
#DTS2012022002232 wangwenbo 20120220 end
```

我在当时移植4.1的时候，就被这些“注释”搞得头晕眼花。不觉得这些都是些天书么？他们试图想代替版本管理软件，来展现一段历史。

在我们的一些老代码中，也有很多类似的注释，一连几个begin，然后不知道哪里end，然后一行代码被围上了好几个begin。看代码的人不断滚屏。我当新员工的时候就问，为啥要这么搞？凭啥要这么搞？被告知，和代码的时候要方便点。

后来，当我真正用到IBM的Clearcase的时候，我才知道，为啥这样要“方便一点”。Clearcase，至少在7.x版本，还是没有一个元commit的概念。他只能每个文件生成一个节点，对每次commit，都会给不同的文件形成多个节点。每次commit，其实是对几个文件的commit，之间没有丝毫的关系。所以一旦上库后，你想知道某个问题是怎么修改的，你光看一个文件，没法找到其他文件的修改！而SVN，GIT这些，每次commit都是一个完整的集合，则不存在这些问题。所以，在这样老土的版本管理软件下，只能采取类似的注释，企图能够快点找到所有的修改。特别是CMO合并问题单的时候！

但这样，真的是相当的不安全。所以我采取的做法是，我在问题单中贴上我所有的修改，但不在代码中做注释，你想合并问题，自己看我的问题单，理解了，然后再合并。省得由于我的begin end没有整对，到时候怪到我头上。

要准备睡觉了，后面再写一些源代码版本管理之类的小文。最近突然有兴趣写点啥……呵呵。
