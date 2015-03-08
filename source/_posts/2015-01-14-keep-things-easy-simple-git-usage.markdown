---
author: hesicong
comments: true
date: 2015-01-14 15:01:02+00:00
layout: post
slug: keep-things-easy-simple-git-usage
title: 别把事情搞复杂了
wordpress_id: 3485
categories:
- 软件
tags:
- git
- pull
- rebase
---

2012年底，当时还在华为的时候，新的项目面临非常大的时间压力，要求在一个月之内搞定最初的代码，包括编译、链接。状况是成都、武汉、深圳三地几十个开发人员共同合上上十万的代码，而代码管理软件用的是[ClearCase](http://en.wikipedia.org/wiki/Rational_ClearCase)(如果你不知道，那么看看吧)！

ClearCase用的是悲观锁机制，一个人锁了文件别人无法取出来；没有原子commit，意思是每个文件只能单独提交，无法跟踪同样的修改，甚至你能只能提交.h而.cpp被人锁了没法提交，大家自然也无法编译；分布式的环境造成ClearCase异常的慢，大家都在做徒劳的加班，经常有人大骂别个又锁了文件还没释放，经常我的提交被别人干掉，我得重来。

我乐观的预计，照这个速度，7*24的话3个月都搞不定。那我们来试试别的方式吧。

### 简单命令干大事

我大胆的给PL、PM建议，用git吧。领导说git大家不会，会不会很麻烦，我们还是用SVN吧，大家都用过，公司也有服务器啥的。其实大家最多就听过SVN，对于git这玩意儿鲜有人知。而我之前已经在本地一直用git很长时间，并结合git-svn管理代码了。

对于这种分布式、大量的merge和慢速的网络的情况下，git的分布式、自动merge、增量式提交直接解决这些要害问题。当然对于新鲜的事物，大家都很谨慎，而且对于华为这样的企业，对于源代码之内的东西都是非常重要的。当然，要推动大家，必须要让大家会用，并且要非常简单。Project Leader决定让我冒个险。

还好TortoiseGit和TortoiseSVN都差不多，有些开发人员用过TortoiseSVN管理过文档，所以还是有一定的基础。我花了数个小时搭了一个http git服务器，当然用的是最简单的。然后花了1个小时时间给几十个人讲这个怎么用。当然原理只能一概而过，十多分钟吧。然后重点是实践，那么怎么样最简单？我总结了几个步骤：* 本地修改
  * commit
  * push
  * 不成功则pull，解决冲突再push

别去理会其他的东西，坚持这几个步骤绝不会出错，我们都在一个master上工作。我建了一个临时的仓库，大家试了一把，果然好用。规则简单、速度快、大部分情况不用手工解决冲突。

就这样，三个团队一个月轰轰烈烈的完成了所有的任务，而且质量好。由于这个合上去的版本只能作为临时的选择，所有的代码最终合入到了SVN中，彻底消灭了ClearCase。当然这期间版本软件转换还是很费力，申请权限、申请空间、教会大家怎么用。

### 懂得越多，可能事情就变得复杂起来

最近遇到的pull --rebase的问题就是个麻烦事。项目组之前约定说merge的commit太难看了，用pull --rebase能够解决问题。试了试，果然好用。直到突然用了分支以后，出现了奇葩的问题。

创建一个分支，然后做一些修改，例如C1，C2。然后merge到master上，创建一个C3，这时候再master分支上pull --rebase，瞬间就傻了。你会发现C3不见了，C. C2在master上重建为C1',C2'，有时候如果当初merge的时候有冲突，这时候你还得来一遍。最后，你的分支就变成了一个unmerged分支，而且永远没法合回去了。

第一次遇到这个坑以后，我以为是自己的问题；第二次遇到后，决定研究一下。这里有一些资料：

[http://stackoverflow.com/questions/6248231/git-rebase-after-previous-git-merge](http://stackoverflow.com/questions/6248231/git-rebase-after-previous-git-merge)
[https://sethrobertson.github.io/GitBestPractices/](https://sethrobertson.github.io/GitBestPractices/)

其实rebase的优点主要是没有auto merge的那个commit，带来的副作用远比这个洁癖多([https://sethrobertson.github.io/GitBestPractices/](https://sethrobertson.github.io/GitBestPractices/))：

> A specific circumstance in which you should avoid using git pull --rebase is if you merged since your last push. You might want to git fetch; git rebase -p @{u} (and check to make sure the merge was recreated properly) or do a normal merge in that circumstance.

> Another specific circumstance is if you are pulling from a non-authoritative repository which is not fully up to date with respect to your authoritative upstream. A rebase in this circumstance could cause the published history to be rewritten, which would be bad.

> Some people argue against this because the non-final commits may lose whatever testing those non-final commits might have had since the deltas would be applied to a new base. This in turn might make git-bisect's job harder since some commits might refer to broken trees, but really this is only relevant to people who want to hide the sausage making. Of course to really hide the sausage making you should still rebase and then test each intermediate commit to ensure it compiles and passes your regression tests (you do have regression tests, don't you?) so that a future bisector will have some strong hope that the commit will be usable. After all, that future bisector might be you.

> Other people argue against this (especially in highly decentralized environments) because doing a merge explicitly records who performed the merge, which provides someone to blame for inadequate testing if two histories were not combined properly (as opposed to the hidden history with implicit blame of rebase).

> Still others argue that you are unable to automatically discover when someone else has rewritten public history if you use git pull --rebasenormally, so someone might have hidden something malicious in an older (presumably already reviewed) commit. If this is of concern, you can still use rebase, but you would have to git fetch first and look for "forced update" in that output or in the reflog for the remote branches.

> You can make this the default with the "branch.<name>.rebase" configuration option (and more practically, by the "branch.autosetuprebase" configuration option). See man git-config.


虽然这里给出了解决问题的办法，但实在是太麻烦了。如果你容忍不了这个auto merge，那你得冒着搞坏别人或者搞垮自己的风险。当然如果你说我就喜欢折腾，我是git大牛，我就喜欢这么折腾。那我也没办法。

回过头来，如果考虑到整个团队的水平、学习曲线及平时的管理难度，我认为还是先暂时用着最原始的auto merge吧，虽然有几个难看的commit。

总之，我们用工具是为了生活更美好。如果这个事情把问题搞的越来越复杂复杂，或者为了炫耀自己熟知这个命令的n多个坑，而你不知道，我认为这和回字的三种写法(回、囘、囬)是一样的道理。

一般来讲，从哲学上说，简单的东西一般是对的([http://en.wikipedia.org/wiki/Occam's_razor](http://en.wikipedia.org/wiki/Occam's_razor))。最后，分享一个Atlassian Blogs上的一篇博客([http://blogs.atlassian.com/2013/10/git-team-workflows-merge-or-rebase/](http://blogs.atlassian.com/2013/10/git-team-workflows-merge-or-rebase/))作为结尾吧。没时间看的，你可以只看Decisions, decisions, decisions: What do you value most部分。
