---
author: hesicong
comments: true
date: 2008-03-28 05:49:05+00:00
layout: post
slug: solution-mfc-program-error-out-of-the-question-when-the-destructor
title: 解决MFC程序退出时析构出错的问题
wordpress_id: 342
categories:
- 其他
---

弯管机的程序确实很神奇，之前是sgCore库初始化不正常，现在退出的时候又出了问题，问题还很诡异，出现在析构退出堆栈的时候，问题很复杂，也没有找到原因。为了让最后不弹出那个可恶的出错对话框，我想了一个狠招，直接在析构函数出错之前将应用程序KILL了，问题解决了，剩下的问题就是为什么出现这个问题呢？
