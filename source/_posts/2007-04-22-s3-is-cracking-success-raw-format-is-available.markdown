---
author: hesicong
comments: true
date: 2007-04-22 07:11:55+00:00
layout: post
slug: s3-is-cracking-success-raw-format-is-available
title: S3 IS破解成功！可用RAW格式
wordpress_id: 186
categories:
- 生活
- 硬件
---


闲来无事逛了逛太平洋电脑网的BBS，本来是想看看转接筒和UV镜的相关信息的，结果以外的发现S3 IS的破解，可以存储为RAW格式、显示电量(郁闷的是S3IS原版系统没有)、显示时间、显示柱状图等等功能，而且整个破解过程不用刷Flash，很安全不会造成保修的问题。佩服俄罗斯人的强悍的破解的同时拿出相机，即可破解。

格式化，安装一个程序，然后按相机上面的几个键就搞定了。而且更为神奇的是可以Make it bootable，也就是说在卡的写保护锁定的情况下开机，就可以运行补丁，而在关闭写保护以后，补丁就不会运行，机器恢复原样！！！强悍啊～～～～我原来以为卡上的写保护是一个硬件级别的保护措施，现在看来只是给了软件一个“我已经写保护”的提示而已，并不是完全不能写。

打开菜单，测试了拍摄几张RAW格式的照片，然后转换成DNG格式以便PS CS3能够识别。照的几张照片都是光源比较亮的情况下照的，这是为了测试RAW格式和JPEG之间的区别。很明显，RAW格式在调节曝光的时候体现出了很强的优势，在增加1级曝光的情况下暗处的细节就显现出来了，而且亮处的细节也不会丢失，这是因为RAW格式记录了每位记录了10bit的数据(S3 IS机型)，比JPEG的8bit数据得到的细节就更为丰富，全然没有JPEG照片丢失暗处细节的痛苦。

后来看了一些相关的资料，知道相机用的是硬件是一个DSP+ARM，操作系统是VxWorks。正如我的SIEMENS 65手机的ARM7芯片一样，现在的数码产品的芯越来越通用化，留给嵌入式高手开发的余地也越来越大了，当然，最终给用户带来的欢乐也越来越多！

下面的两张图，这两张图还不能展现RAW格式的魅力，用PS CS3直接打开RAW格式然后进行曝光等处理，才能看到RAW格式的优势！

图1：相机JPEG格式，PHOTOSHOP里面加+1级曝光。白炽灯的颜色在相机白平衡处理以后颜色已经不正常了。
[![img_12931small.jpg](/images/others/thumbs/thumbs_img_12931small.jpg)](/images/others/img_12931small.jpg)

图2：相机RAW格式，只+1级曝光，其他全部默认。可以看出这张图片很接近于拍摄时的环境，白炽灯的淡黄色看起来很舒服。
[![img_12931smallraw.jpg](/images/others/thumbs/thumbs_img_12931smallraw.jpg)](/images/others/img_12931smallraw.jpg)
