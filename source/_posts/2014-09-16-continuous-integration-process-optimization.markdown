---
author: hesicong
comments: true
date: 2014-09-16 12:15:24+00:00
layout: post
slug: continuous-integration-process-optimization
title: 持续集成流程优化
wordpress_id: 3329
categories:
- 软件
tags:
- CI
- jenkins
- 持续集成
---

持续集成有时候成了一种固定的模式后，就难以摆脱定式思维，其实我们还有很多可优化的空间。

### 习以为常的CI流程

我们通常习惯的CI模式基本上如下图所示，运行于Jenkins，开发基于Maven Spring AngularJS等。

![](/images/2014/09/Untitled-Diagram-1.png)](/images/2014/09/Untitled-Diagram-1.png)

各个步骤的用途如下：

* BUILD & UNIT TEST：构建和单元测试，生成后续步骤所需要的artifacts。
* INTEGRATION TEST：集成测试外部依赖。
* END TO END TEST：有时候称之为Functional Test，测试整个系统的功能，通常运行于Windows+Jetty环境运行。成功后进行部署。
* DEV ENV：开发人员的测试环境，运行于Linux+Tomcat环境。
* SYS ENV：测试人员的环境，运行于Linux+Tomcat环境。
* UAT ENV：通常是business的人进行用户验收测试，运行于Linux+Tomcat环境。

这样的步骤确保了在END TO END TEST之后的代码都是已经测试过，品质量好的代码。但我们还是发现了其中存在的若干问题。

### 问题一：DEV环境通常闲置

在我经历的两个项目中，开发人员一般都在本地进行调试好了以后，不会在DEV环境再进行调试。DEV环境的唯一用途恐怕就是测试部署是否正常。所以一般情况下都闲置，利用率较低。

### 问题二：END TO END测试的环境和真实产品环境不匹配

在目前我们的END TO END TEST阶段，主要是用本地的Jetty来运行测试，整个测试过程都是在CI的Windows服务器上进行，和真实环境中的Linux+Tomcat环境还是有差异。由于环境的差异，虽然测试可能通过，但还是可能遗留一些和环境相关的问题。

### 问题三：END TO END测试出错后，不容易找到问题来源

由于END TO END通常都是比较脆弱的环境，第三方的依赖的不稳定和测试工具的不稳定都很容易使之失败。失败后，开发人员通常会查看Jenkins上的日志，然后分析原因。然而这些原因非常难找，例如Protractor的Stack就基本上没太大用户，assert失败后就只能靠猜测。对于很多概率性的问题或者本地环境没有的问题，需要反复的checkin代码进行尝试，更难进行调试。

另外END TO END处于比较后期，一旦发现问题需要修改某些代码然后在进行测试，将会变得异常的漫长。

### 解决方案：重新布置CI流程

我们仅仅需要做一点点小的修改，将DEV环境移到END TO END之前，就能解决上述的问题

[![改进的CI流程](/images/2014/09/Untitled-Diagram-2.png)](/images/2014/09/Untitled-Diagram-2.png)

和之前的CI流程相比，DEV会在集成测试之后，直接进行部署。END TO END的测试脚本将会指向于DEV环境进行测试而不是之前的本地Jetty环境。

### 解决的问题

* 问题一：DEV环境通常闲置。

> DEV环境将作为END TO END环境的测试对象，利用率更高。

* 问题二：END TO END测试的环境和真实产品环境不匹配

> DEV环境运行于和的Production一样的环境，END TO END测试更为真实可靠。

* 问题三：END TO END测试出错后，不容易找到问题来源

> DEV环境可以直接访问，并且已经部署，出错后，可在本地将END TO END测试指向DEV环境，进行测试。如果是测试代码问题，修改后马上可以重新测试；如果是产品代码的问题，修改提交后只需要重新单元测试及集成测试即可，速度比之前会更快。

### 带来的问题

这样做当然也带来一个问题：对比之前，此时的DEV环境将不是可靠的一个环境。但DEV环境本来不是给开发人员玩的环境么？所以我个人认为这不是一个问题，我将其命名为TEST DEV ENV以便和之前的DEV ENV进行区分。
