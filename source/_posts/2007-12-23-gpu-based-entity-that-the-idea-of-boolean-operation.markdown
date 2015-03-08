---
author: hesicong
comments: true
date: 2007-12-23 16:32:37+00:00
layout: post
slug: gpu-based-entity-that-the-idea-of-boolean-operation
title: 基于GPU的实体布尔运算的想法
wordpress_id: 286
categories:
- 其他
tags:
- opengl
---

最近看了看NVIDIA的DX10的演示，发现Geometry Shader的用处太大了，就忽然联想到是不是可以用来做实体运算。经过一番搜索，找到一些有用的资料，在这里做个笔记：

[OpenGL Geometry Shader Marching Cubes](http://www.icare3d.org/content/view/50/9/)
[Polygonising a scalar field](http://local.wasp.uwa.edu.au/%7Epbourke/geometry/polygonise/)

看考完试了把这个东西拿来做一做，用DirectX来做吧，然后再在OpenGL上面来做。OpenGL的优势在于XP下面都能用Geometry Shader。
