---
author: hesicong
comments: true
date: 2013-12-24 12:09:58+00:00
layout: post
slug: steps-to-angularjs
title: 爱上AngularJS的几个阶段
wordpress_id: 3287
categories:
- 软件
tags:
- angularjs
---

上个月去休假前，在研究用Cordova写一个手机的软件。在研究UI表现方面，花了一些时间和精力试用了一下AngularJS。这个玩意儿在**Thought**Works技术雷达上出现过好多次了，公司CTO也强烈推荐这玩意儿，所以就用了。花了一小会儿时间看了AngularJS，然后就慢慢地用上了。

**第一阶段：爽**

AngularJS入门总是很快，几钟就可以搞定一个简单的页面。官网首页的Demo已经很强大，像我这种jQuery都不怎么搞的会的，绝对会立刻放弃jQuery投入AngularJS的怀抱，因为你只需要搞定一个model和controller，其他的DOM上的事情就不关心了。

这阶段，主要的就是不断的体验这种directive，爽的一塌糊涂。官网的Tutorials一步一步的做下去，很快就能体验到AngularJS提供的大部分功能和e2e功能。当我开始半灌水的时候(其实当时只有一点点水)，新的项目开始了。经过一个星期的反复折腾、沟通和忽悠，终于在最后阶段用一个Showcase说服了客户，决定在新的项目中使用AngularJS。

**第二阶段：痛苦**

新的需求的到来，让一个完全无AngularJS经验的团队显得很无助。各种奇葩的问题，让我们只有不断的google寻求答案。答案当然也有一些，当面对directive、link等名词时，才发现原来AngularJS的坑那么深。还好办公室[扎爷](http://www.cnblogs.com/whitewolf/)有很深厚的AngularJS使用功底，为项目初期的疑难问题做了很多的工作。以下三篇文章都是我们项目中的问题的解决。* [angularjs ng-option ie issue解决方案](http://www.cnblogs.com/whitewolf/p/3464053.html)
  * [angular ng-model类型格式转化](http://www.cnblogs.com/whitewolf/p/3474990.html)
  * [angularjs组件之input mask](http://www.cnblogs.com/whitewolf/p/3475687.html)

这阶段主要是折腾AngularJS的各种文档，google问题。有时候像无头的苍蝇到处乱撞。要顺利的度过这个阶段，得静下心来，系统的再看看AngularJS相关的东西，[《用AngularJS开发下一代Web应用》](http://product.china-pub.com/3768727)这本书也可以买来看看。这本书比较浅显，稍微熟悉AngularJS的一天就能看完。听说网上已经有开源版本出来了，不愿意花钱的也可以搜一下。

**第三阶段：起死回生，自力更生**

当你发现其实AngularJS或者AngularUI提供的各种directive都不能满足你的要求的时候，该到了写directive的时候了。这个有点类似于以前玩WinForm，当所有的控件都不满足要求的时候，就得自己写控件了。这个玩意儿我认为是AngularJS比较核心和困难的部分。

最近一个多星期我都在写validate的directive，也遇到了一些问题。但由于对AngularJS的用法更为了解以后，解决问题起来就显得稍微有了一些谱。

**第四阶段：深入理解，融会贯通**

这阶段，我认为还没达到，由于项目的需求还没要求能力达到这种程度。所以要达到这个阶段，我认为需要自己去努力，在AngularJS的源代码中去寻找真理了。

加油吧！
