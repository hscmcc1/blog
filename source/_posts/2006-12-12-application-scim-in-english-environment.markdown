---
author: hesicong
comments: true
date: 2006-12-12 03:11:19+00:00
layout: post
slug: application-scim-in-english-environment
title: scim在英文环境中应用
wordpress_id: 103
categories:
- 软件
tags:
- ubuntu scim
---

edit /etc/profile and add these:

```
export LANG=en_us.UTF-8
export XMODIFIERS="@im=SCIM"
#export GTK_IM_MODULE=xim
export GTK_IM_MODULE=scim
export QT_IM_MODULE=scim
export XIM_PROGRAM=SCIM
export XIM=SCIM
export LC_CTYPE="zh_CN.gb2312"
```

```
su:scsioffice ~ # export |grep -i scim
declare -x GTK_IM_MODULE="scim"
declare -x QT_IM_MODULE="scim"
declare -x XIM="SCIM"
declare -x XIM_PROGRAM="SCIM"
```