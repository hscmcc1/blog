---
author: hesicong
comments: true
date: 2009-06-23 06:26:30+00:00
layout: post
slug: fix-virtualbox-100-cpu-usage
title: Fix VirtualBox 100% CPU Usage
wordpress_id: 1848
categories:
- 其他
tags:
- 100%
- cpu
- intel
- smp
- vbox
- virtualbox
- vt-x
---

I was using VirtualBox in Fedora and Vista but it happened to use all CPU resources in guest machine and the VM seems very slow. You can observed this problem by opening the task manager and see the CPU utilization at 100%, and core cpu usage is around 90%.

I searched about this problem but found only the questions and no useful answer. I even tried the VirtualBox 3.0 beta1 which supports SMP in VM but ended up facing blue screen of Vista guest. Seems there is no solution.

 I just remebered VirtualBox said its software virtualization is better than Intel VT-x, should I try to disable VT technology? I uncheck the default setting of VT and reinstalled the guest system. All things goes smoothly!

But unfortunately VirtualBox seems slow to me and I will try KVM instead.
