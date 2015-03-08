---
author: hesicong
comments: true
date: 2007-04-22 09:28:25+00:00
layout: post
slug: howto-fix-geforce-4-420-go-black-screen-problem
title: HOWTO:Fix geforce 4 420 Go black screen problem
wordpress_id: 187
categories:
- 硬件
tags:
- ubuntu
---

Post at http://ubuntuforums.org/showthread.php?p=2501546#post2501546

I've been having trouble with my laptop geforce 4 420Go video card on NVIDIA driver. The problem, like most other 420Go users, is the BLANK SCREEN problem.
The problem appear on my BENQ Joybook 3000 is like this:
After I installed the restricted driver and reboot , it's blank screen when I login but I can hear the sound. I type my user and password, it seems OK and login. But I still get blank screen.
I can get to the terminal when I press CTRL+ALT+F1 and I can switch back again by pressing CTRL+ALT+F7.
I make sure that Xserver and the video driver are both working fine, but video driver may not initialized the LCD display!
So I google again and I found this article: https://bugs.launchpad.net/ubuntu/+s....20/+bug/96430
This is a bug report about this problem. I read about this L--O--N--G discussion and near the bottom of the article, I see jdriessen( said on 2007-04-12) told us :
"in response to my previous post
those of you having similar problems if you have a Geforce4 420 Go 32MB.
the 9631 driver is looking for a CRT monitor and not and LCD (those of you having the problem on LCD monitors)
to fix this:
edit your xorg.conf
go to Section "Screen"
below defaultdepth, add: Option "UseDisplayDevice" "UFP-0"
this should fix the blank screen issue on LCD displays."

I tried it ,but it not work!
And again, very lucky, I googled for keyword ""UseDisplayDevice" I found some discussion about it. In a Chinese forum, some one managed to let even Geforce 2 MX to work with beryl!
He said add Option "UseDisplayDevice" "DFP" to xorg.conf and reboot.

I tried it, YEAH! It works! NVIDIA logo appears when I before my login(I set Option "NoLogo" "False"), and compiz in ubuntu 7.04 works very well!!!!

So, if you can hear the login sound, if you use 96.31 driver, if you use ubuntu 7.04

TRY ADD
Option "UseDisplayDevice" "DFP" or
Option "UseDisplayDevice" "DFP-0"
to help this NVIDIA Driver find your first LCD display!

