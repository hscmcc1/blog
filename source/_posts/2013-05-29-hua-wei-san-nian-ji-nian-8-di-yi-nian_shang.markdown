---
author: hesicong
comments: true
date: 2013-05-29 15:27:34+00:00
layout: post
slug: hua_wei_san_nian_ji_nian_8_di_yi_nian_shang
title: 华为三年纪念——(8)第一年上
wordpress_id: 3258
categories:
- 生活
---

一年的事情，回想起来好长。

转正后享受着早上8点到9点的弹性，也就用不着那么早起床了。但如果每天都这么玩的话，每天的平均工时自然是不够的。所以，来得很晚的同事，为了将每天1个小时补足，只能. . 4晚上多干两个小时左右。弹性工作当然是针对所有的研发员工，但要享受这个待遇，坐班车是不可能了。租房的、开车的、抑或走路骑车的，都可以享受到这样的待遇。我总想下午早点走，所以每天早上弹性时间不久，最怕的是下午下班时，突然接到电话，说晚上要干啥干啥。

一个人的力量，是无法撼动整个地球的。大家是一个团体，你的效率在搞，和一群效率低下，只想着晚上加班的人在一起，只得适应这样的生活。

转正后，交接的两位同事也会深圳了。剩下的项目组的几个人奋斗。第一次经历产品TR5发布前的测试，真是折腾死人。一堆一堆的问题接踵而至，似乎这些问题之前都隐藏了起来。越到后面，修正问题的成本越高，每修改一个问题，都要经历自测试、代码检视、手工测试，然后提交。其实内部的DTS系统的流程还是做的比较规范的，每个步骤都有，用起来还是可以，只是稍显麻烦。虽然经历了这些步骤，但我接手的那两个模块由于太复杂，有很多的特殊条件，也没任何的Low Level Test，造成质量很难评估。

有一次，刚刚修改了问题上库，成都测试没问题，但刚刚要发布，深圳那边爆出了一个严重问题，由于我的修改导致。其实我就修改了一点点，牵一发动全身的这种感觉，你会觉得毛骨悚然。当时交接的同事还和我一起写的那些代码，每一行都是他看过的。居然还出现问题，那到底是我技术有问题，还是代码本身有问题？我更相信后者。我总结的只要会if..else，在这里就能混下去了，有些还混得很好。

我最讨厌的是出了问题以后，领导过来分析原因，扯上一堆人，最后得出一个“低级错误”。什么变量赋值又出问题啦，数值为BYTE的被赋予了WORD型造成截断啊啥的。总是批评说我们不认真、不仔细，不遵守“编程规范”等等。其实，人本来就是一个容易出错误的高级动物而已。明知道变量类型有可能改变，为什么还在用匈牙利命令法，并且最新的编程规范已经不推荐使用了。更可怕的是很多人以这个作为判断类型的。如果一个行代码这么写：

void func(BYTE byInput)
{
       BYTE byVar = byInput;
}
说不定哪天接口改成了WORD，虽然你修改了头文件和实现，但恐怕很难再去修改这个byInput，造成迷惑。后续代码检视时，往往可能看中间的部分，如果没看函数头，则很容易认为你这是对的。所以，我的代码中不写匈牙利命名。以至于PL批评我和大家的风格不统一，每次代码检视都给我提出。后来也为了不麻烦，还是用了一下。

上半年，加班不算太多，但部门已经开始慢慢的衰退。我刚才进公司时，导师给我介绍产品时，就说这个产品已经是夕阳了。每次大领导开会时，都会提到现金牛几个字来鼓舞人心。前几次，我还听得兴奋不已，后来再听的时候，只觉得是耳边风罢了。

这一年，公司利润很好，分红很高。在天府软件园B7楼下，还没发奖金和股票，一群售楼的就每天轮番轰炸。一些资历老一些的同事拿了分红，买个车。可见，在华为干得比较久的真是高富帅一群。但对于我们应届毕业生来讲，一切都是浮云，第一年拿到一点点奖金，只能途个羡慕。

上半年加班最晚的一次到了凌晨2点过到家。年中，产品过了TR6，没啥事情干了。. 7月正是慢慢火热的时候，但我感觉还比较清闲。这段时间就用来读书，我喜欢看书，看《重构》一书。

机遇总是给已经准备好了的人。PL找到我说有一个顾问要过来，问我有没有时间和他一起将我那个糟糕的模块做一些重构。我说有啊，PL也相当支持我的工作，至今我都感激他给了我一周的时间，能够和敏捷顾问一起坐下来，全职的投入到重构中。顾问也觉得幸运，我能有那么多的时间。他说很多时候只能到一个项目组待上一两天，零零散散的，总被员工的一些杂事或者需要定位问题等打断。

由于老模块的腐化很严重，一个“模块”，一个.CPP就1万多行代码，你说不乱么？我们对整个CPP做了分析，手工拆出来几个函数变成类。然后我们针对两个高达500多行的两个复杂函数，也是很容易出错的函数进行重构。对于这种大型代码，《重构》一书中提到边覆盖边重构的思路，很爽。我也是拿了书，和顾问一起，他用的方法我叫他在书中帮我提示一下，每天回家后再仔细看一下。

这样，我们用Google Test和Google Mock对函数进行一次一次的打桩、测试、重构、测试，将这两个函数拆成了很多个小函数，原来庞大的一个函数变成了10行以内函数，清晰度很高。每个小函数集中功能。顾问对函数的命名也颇为讲究，总是反复和我讨论抽取的函数的职责，反复研讨函数名。对于复杂逻辑，我们写真值表，将以前看起来复杂不堪的逻辑简化成了几个条件或者函数，一目了然。代码分支覆盖率100%，每一个函数都测试到了，重构起来相当放心。这种体验真是棒极了！

但，这个重构后的代码，并没有用于产品中，理由是大家认为这些代码没有经过长期测试，放上去怕出问题。好吧，这次的经历对我来说难得，这个星期的学习，真正让我的软件能力提升了一个档次，知道了重构的方法，为我后续的成长指明了道路。
