---
author: hesicong
comments: true
date: 2009-05-10 02:18:05+00:00
layout: post
slug: esl-podcast-downloader
title: ESL podcast downloader
wordpress_id: 1747
categories:
- 软件
tags:
- downloader
- esl
- eslpodcast
- python
---

When I first met ESL podcast, I found it was very useful for your listening and it expanded your vocabulary and your communicate skills. On the website, you can download all of the podcasts and read the scripts online by free. There are now 670-odd podcasts on the website, so it is a huge treasure for English learners.

I was wondering if I could create a tool and download them all at once including the audio index and the script and then convert them to a plain text form in order that I can listen to the podcast and read the script on my phone. To realize my idea, I wrote a small program by python under Linux.

Download it:

[download id=32]

How to steps:

1. Be sure you are using Linux or Unix system or Cygwin system.
2. Make sure you have installed Python 2.x
3. Copy the script to any folder you like.
4. Make sure you have right to create a sub-directory under this folder. My script will create a folder called "eslpod" to store the content.
5. Run this script.

You will see the script start to fetch the pages and download the podcasts. Downloaded files will store under "eslpod" folder and grouping by the tag. Under every tag, you could see the script with .html suffix and podcast with .mp3 suffix.

You can interrupt the downloading process by kill the process.

DISCLAIMER:* Use this script at your own risk.
  * This program may fail if the site was changed.
  * Respect the authors' work and do not distribute without their permission.

And at last, be happy and confidence to study English!
