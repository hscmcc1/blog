---
author: hesicong
comments: true
date: 2009-08-30 15:26:38+00:00
layout: post
slug: metaweblog-synchronization-problems-blog
title: metaWeblog同步博客遇到的问题
wordpress_id: 1992
categories:
- 软件
tags:
- blog sync
- metaWeblog
- python
- xmlrpclib
- 博客同步
---

最近有一个迫切的需求，需要同步几个博客。目前我有的博客是本站，还有hesicong.cnblogs.com和www.csdn.net/hesicong共三个。其后两个由于各种问题长久以来没有得到更新。我的需求就是写一个软件能够将wordpress的文章同步到cnblogs和csdn(成为次博客)。对于主次博客都有的文章修改次博客为主博客内容，对于次博客没有的文章按照时间新建一个。从而实现博客同步。

技术上使用metaWeblog就可以实现上述目的，选用python的xmlrpclib可以方便的进行xmlrpc操作。做一个控制台的小程序足够了我使用了。

然后开始技术实验，发现：

1. wordpress支持metaWeblog很好，可以实现所有的功能。从wordpress可以通过metaWeblog.getRecentPosts函数得到所有的文章。
2. cnblogs也支持metaWeblog，也支持的很好。cnblogs也支持我的语法高亮。但遗憾的是：第一：metaWeblog.getRecentPosts函数最多能够返回100个文章。而我的cnblogs目前有230篇文章，很显然，cnblogs限制了文章数量；第二：metaWeblog.newPost函数即便Post结构中有dateCreated，但cnblogs的主界面中依然按照当前时间计算，造成文章时间对不上号，顺序混乱。
3. csdn就是个垃圾，metaWeblog表面上支持，暗地里出问题。metaWeblog.getRecentPosts,metaWeblog.editPost都无法用，提示User not exist。仅有metaWeblog.newPost可以用，但csdn的blog的语法高亮无法用，页面很难看。

所以，想实现我的目的通过metaWeblog看来是没希望了。除非cnblogs调整文章数量限制，csdn希望从垃圾变成战斗机了。

附我写的一个测试代码，不完善，仅作为参考：

``` python
import xmlrpclib

class Metablog:
    def __init__(self, url, username, password):
        self.username=username
        self.password=password
        self.url=url
        self.server=xmlrpclib.ServerProxy(url)
        self.posts=None

    def getAllPosts(self):
        print "Getting all posts from "+self.url
        self.posts=self.server.metaWeblog.getRecentPosts('', self.username, self.password, 9999999)
        print "found "+ str(len(self.posts)) +" posts"
        return self.posts;

    def getAllPostTitle(self):
        if self.posts==None:
            self.getAllPosts()

        ret=dict()
        for post in self.posts:
            ret[post["postid"]]=post["title"]

        return ret

    def getPost(self, id):
        for post in self.posts:
            if post["postid"]==id:
                return post

        return None

    def newPost(self, post):
        self.server.metaWeblog.newPost('', self.username, self.password, post, True)

    def editPost(self,postid, post):
        self.server.metaWeblog.editPost(postid, self.username, self.password, post, True)

    def delPost(self, postid):
        self.server.metaWeblog.deletePost('',postid, self.username, self.password, True)

def syncBlog(b1, b2):
    b1Titles=b1.getAllPostTitle()
    b2Titles=b2.getAllPostTitle()
    for key1,value1 in b1Titles.iteritems():
        print "Blog1 title: "+value1
        for key2,value2 in b2Titles.iteritems():
            print "tBlog2 title: "+value2
            if value1==value2:
                print "Syncing, blog 2 postid="+key2
                b2.editPost(keys2, b1.getPost(key1))
                break

        print "Blog2 has no article equal to title :"+value1
        print "Add new "
        b2.newPost(b1.getPost(key1))

    print "Done sync"


wpBlog=Metablog("主站地址", "用户名", "密码")
cnBlog=Metablog("从站地址", "用户名", "密码")
syncBlog(wpBlog, cnBlog)
```