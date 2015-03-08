---
author: hesicong
comments: true
date: 2004-07-12 14:57:44+00:00
layout: post
slug: careful-video-card-bios-update-tool-nvflash-change-killer
title: 小心！显卡BIOS刷新工具Nvflash变杀手
wordpress_id: 1165
categories:
- 硬件
---

小心！显卡BIOS刷新工具Nvflash变杀手

2004-01-08■贺思聪■电脑报

前些年流行的CIH病毒曾使无数的主板毁于一旦，你有没有想过采用NVIDIA显示芯片的显卡也受着这样的威胁。

威胁就是来自Nvflash——NVIDIA公司所有显示卡BIOS的通用刷新工具。不少报刊都提到这个工具只能在DOS下运行，但经过笔者验证，4.42版本能够在各个版本的Windows中成功保存、更新显卡BIOS。这给我们带来了极大的便利，省去了制作启动盘的麻烦，但同时也给我们带来了潜在的威胁。

在帮助中看到Nvflash的e参数可以清除EEPROM(电可擦除只读存储器)的内容。由于4.42版在 Windows下也能存取显卡BIOS，所以也可以直接清除显卡BIOS内容。在默认情况下Nvflash能够发出提示音，但使用s参数后就处于静音模式。这样如果联合参数e、s和2，还没有等你反应过来显卡BIOS就被清空了。但此时不要重启电脑，用Nvflash把正确的显卡BIOS文件刷进去，同样不会出现什么问题。但假如你根本不知道，当重新启动电脑的时候你就只能面对黑糊糊的屏幕发呆了。更糟糕的是如果加上参数y，那么在擦除完了之后就立刻重启，根本没有挽救的余地了。

如果你不小心执行了经过伪装并且带参数的Nvflash，那这个东西将成为名副其实的N卡杀手。这里提示大家，可以在系统的“命令提示符”下运行Nvflash -c来检查显卡EEPROM是否在它支持的范围之内，如果是的话就要加以提防了。如果你真的成为了幸运儿也无所谓，找一张PCI显卡引导电脑再刷新坏显卡的BIOS就行了，具体步骤不再赘述。
