---
author: hesicong
comments: true
date: 2014-03-15 12:15:59+00:00
layout: post
slug: how-to-install-gitlab
title: 怎么安装GitLab
wordpress_id: 3295
categories:
- 其他
---


>  UPDATED: Another way is to install using docker, see [here](https://registry.hub.docker.com/u/sameersbn/gitlab/)

看了一下GitLab的安装方法，就被打败了，后来发现了一个官方推荐的方法

> GitLab packages (beta) These packages contain GitLab and all its depencies (PostgreSQL, Redis, Nginx, Unicorn, etc.). They are made with omnibus-gitlab that also contains the installation instructions. These packages currently support a reduced selection of GitLab's normal features. For instance, it is not yet possible to create/restore application backups or to use HTTPS.

> GitLab virtual machine images contain an operating system and a preinstalled GitLab. They are made with GitLab Packer that also contains the installation instructions.

> https://gitlab.com/gitlab-org/gitlab-ce/blob/master/README.md#installation

如果是Ubuntu的话，仅需要安装一个package就可以了：


> https://www.gitlab.com/downloads/

如果需要改变GitLab的port，编辑：

> /var/opt/gitlab/nginx/etc

中的listen即可。

初始密码在[这里](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md)找到

>  After the steps below your GitLab instance should reachable over HTTP, and have an admin user with username root and password 5iveL!fe.
