---
author: hesicong
comments: true
date: 2005-04-07 16:02:59+00:00
layout: post
slug: mobile-phone-management-software-develop-experience-on-net-platform-chapter-1-to-3
title: Mobile phone management software develop experience on .Net Platform (Chapter
  1 to 3)
wordpress_id: 1237
categories:
- 其他
tags:
- .net
- pdu
---

Mobile phone management software develop experience on .Net Platform
Chapter 1
Introduction
After studying of many aspects of mobile management software development, I finally had written my Siemens Support Tool using VB.Net. Although I have not really finished it due to time limit, the core is developed well and the interface is OK on the whole. My purpose in studying software development is achieved. I stopped this project on February 12th and I want to summaries the works I’ve done on my winter holiday. So I write out what I have learned, in order to review it later and help some one working on analogous work. Because I’m after-hours programmer, so if you find some mistakes, please contact me! Thanks!

Chapter 2
Why do you design it? What’s your aim?
The one who are using or have used Siemens Mobile Phones will be very pleased at their humanity  design. To my disappointment, the official management software SDS is not well designed. It’s hard to use and has a low speed. The newest version software for 65 platform which is called Mobile Phone Manager seems much better than before, but it takes 120MB space on my tiny hard disk and also has a very low speed.
GhostMobile (GM for short) is a good nonofficial freeware I ever used. But it often stops while transferring files. I guess no time-out error handle was designed. It is also very slow in transferring files, and is not very convenient to manage SMS.
Siemens Mobile Control (SiMoCo for short) is also nonofficial software. It has only 800kb in size but with fast operating speed and powerful functions. But after I used it, I found it is hard to use, it seems be designed for experts, and is not well supported Chinese.
So the final aim is to design software with file transferring, SMS, note management, task management and calendar management.

Chapter 3
Prepare
In the summer holiday of 2004, I was done a part functions for file transferring and SMS. I named it M55 File Transfer Tool at that time. After the alpha publish on Dongbei Mobile Ne t([http://bbs.dbsjw.com](http://bbs.dbsjw.com/)), I found some phones which can’t connected by GM can be connected by my program. But many bugs were found.
The portion of SMS is based on ATC_Command_Set_For_L55_Platform. You can download it from either the official site or my site. It describes AT Command Set for Siemens 55 Platform in detail. Actually, the AT Commands for SMS are the same for different manufacturer. But the portion for file transferring is much difficult to explorer.
No official documents describe how to transfer data between phone and PC. I monitored the whole COM operation by Serial Monitor. But all I found was hex values which confused me a lot! What structure does it have?
I have searched Google for answer and posted questions on CSDN. No one knew this, but a friend gives me some thread, he said that maybe part of Bluetooth protocol. Yes, after my Google search on Bluetooth protocol, I found IrDA Protocol. The IrOBEX protocol attracted me! The hex is well structured according to this protocol.
After my carefully study of the hex values, I found that the phone use OBEX to transfer data. Today, this problem seems somewhat drollness, when I saw the state of serial monitor in manufacture mode of phone changed from GIPSY to OBEX, what am I thinking of at that time?
Gradually, I know how to read and write phonebook, note, and calendar by OBEX. I wrote a OBEX class by VB.Net, but its code was hard to read and modify and with a low efficiency. I finally rewrite this class in my new program.
The new program was started at January 14th, 2005. I almost wrote the whole old program. To me, a beginner, it is a tremendous challenge.
