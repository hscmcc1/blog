---
author: hesicong
comments: true
date: 2007-11-08 23:22:56+00:00
layout: post
slug: assert-carefully
title: 小心Assert
wordpress_id: 268
categories:
- 其他
---

今天弄弯管机的破程序的时候，突然发现DEBUG模式和RELEASE模式下面得到的值不同，很是疑惑于这些微妙的差别，以为是RELEASE模式的优化出了问题。但对比DEBUG和RELEASE，即便将几乎所有编译选项都调整为一样，也不能得出一样的结果。

后来进行了跟踪调试，以外的发现assert(oneFunction())这句竟然在RELEASE模式下面不执行。哦，我当时使用这个assert是为了断言一个函数的返回值为正，而这个函数正好要影响我的数据。当然assert在DEBUG模式下面没有问题，正常的编译运行成功。但在RELEASE模式下面一旦调试到这个assert的上一行，在进行单步跟踪的时候就跳到了assert的下一行。也就是说这个函数根本没有运行。原因就出现在这里。把assert去掉以后，函数就正常运行了。

以后用assert要多加小心了，assert函数返回值的时候也一定要把返回值先赋值给一个变量，再assert，就不会出现这样的问题了。
