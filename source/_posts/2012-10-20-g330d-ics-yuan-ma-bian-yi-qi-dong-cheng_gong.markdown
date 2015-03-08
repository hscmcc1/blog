---
author: hesicong
comments: true
date: 2012-10-20 12:17:28+00:00
layout: post
slug: g330d_ics_yuan_ma_bian_yi_qi_dong_cheng_gong
title: G330D ICS源码编译启动成功
wordpress_id: 3173
categories:
- 生活
---

G330D离CM. CM10不远了，今天搞定了4.0.4在G330D的启动，已经顺利的进入源码编译的系统。
来，说一下怎么搞吧：
https://www.codeaurora.org/xwiki/bin/QAEP/release
这个是ICS的：

> October 17, 2012
> M8625SSNSKMLYA1310
> msm8625
> M8625SSNSKMLYA1310.xml

```
$ repo init -u [git://codeaurora.org/platform/manifest.git](git://codeaurora.org/platform/manifest.git) -b release -m [manifest] --repo-url=[git://codeaurora.org/tools/repo.git](git://codeaurora.org/tools/repo.git)
$ repo sync -j32
```

`[manifest]`填写上面那个xml文件

然后就是漫长的等待……其中`source.android`好像会出错，没关系用

```
repo sync -flj32
```

从本地重新`sync`一个，忽略错误。

等东西下载完了，准备开工。

```
source build/envsetup.sh
choosecombo
```

选`2.debug`

然后选13，`msm7627a`

然后就可以开始`make -j4`了

完成后，系统就做了好，可以在`/out/debug/target/product/msm7627a/`下面看到`system.img`等文件即可。

下面开始编译kernel

下载地址：https://github.com/derekhe/huawei-g330d-u8825d-kernel

请看一下：https://github.com/derekhe/huawei-g330d-u8825d-kernel/blob/master/HOW-TO-BUILD

编译完成后，开始制作`boot.img`

从现有的ROM中，解压`boot.img`

解压工具见：

https://github.com/derekhe/u8825d-bootimg-scripts

然后将kernel打包进去，双wipe，

用`adb`刷`system.img`时，记得要用`system.img.ext4`这个~

然后，就等待吧。

之前我卡在启动画面那个地方，后来修改内核打开了logcat，竟然就进系统了。

那，有什么可以用呢？

相机？~NO~电话~NO，等待你的开发~！
