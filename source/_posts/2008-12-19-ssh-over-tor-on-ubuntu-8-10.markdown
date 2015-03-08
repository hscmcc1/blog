---
author: hesicong
comments: true
date: 2008-12-19 14:23:22+00:00
layout: post
slug: ssh-over-tor-on-ubuntu-8-10
title: SSH over Tor On Ubuntu 8.10
wordpress_id: 1478
categories:
- 软件
tags:
- ubuntu
---

我经常需要使用ssh登录linux主机，今天突然想到如何用在ubuntu linux上在tor网路上使用ssh？[查了些资料](http://blog.kurthbemis.com/2008/08/25/tools-ssh-over-tor-for-secure-and-anonymous-sessions/)，使用如下命令即可：

``` bash
sudo apt-get install connect-proxy
ssh -2 -l SSH_LOGIN_NAME  `tor-resolve SSH_SERVER_NAME.com localhost:9050` -o ProxyCommand="connect -4 -S localhost:9050 %h %p"
```

命令解释如下：

> -2：仅尝试使用V2版的协议
> -l：登录
> `tor-resolve SSH_SERVER_NAME.com localhost:9050`，使用tor网络解析网络地址，防止欺骗
> -o ProxyCommand="connect -4 -S localhost:9050 %h %p"，连接代理。

我日常使用管理主机可以只用：ssh -2 -l USERNAME:SSH_SERVER ` -o ProxyCommand="connect -4 -S localhost:9050 %h %p"即可，就不用进行tor网络的域名解析了。加快了速度。

但说实在的，tor网络用ssh速度确实不够理想～～
