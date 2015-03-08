---
author: hesicong
comments: true
date: 2007-02-28 23:52:40+00:00
layout: post
slug: removed-flash-head-title
title: Removed Flash Head Title
wordpress_id: 144
categories:
- 其他
---

It appears that Flash 9.0 under Linux could not handle transparent Flash which causes my negative tool-bar unclickable. I fixed this problem by adding one line to WriteHeadFlash function in "common/common.js":
function WriteHeadFlash(Path,Width,Height,Transparent){
if(isIE()==false) return;     //If not IE, disable flash head
...
}
