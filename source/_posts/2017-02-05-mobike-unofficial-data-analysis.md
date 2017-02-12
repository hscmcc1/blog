title: 摩拜单车非官方大数据分析
date: 2017-02-05
tags: 
- 摩拜
- 爬虫
- 大数据
---

趁着过年的清闲时光，我抓取了摩拜单车的数据并进行了大数据分析。以下数据分析自1月19日整日的数据，范围成都绕城区域以及至华阳附近（天府新区）内。成都的摩拜单车的整体情况如下：

### 标准车型和Lite车型数量相当

摩拜单车在成都大约已经有6万多辆车，两种类型的车分别占有率为55%和44%，可见更为好骑的Lite版本的占有率在提高。（1为标准车，2为Lite车型）

![单车类型](http://upload-images.jianshu.io/upload_images/4372317-405ce446062c7b91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 三成左右的车没有移动过

数据分析显示，有三成的单车并没有任何移动，这说明这些单车有可能被放在不可获取或者偏僻地方。市民的素质还有待提高啊。

### 出行距离以3公里以下为主 
数据分析显示3公里以下的出行距离占据了87.2%，这也十分符合共享单车的定位。100米以下的距离也占据了大量的数据，但认为100米以下的数据为GPS的波动，所以予以排除。

![出行距离分布](http://upload-images.jianshu.io/upload_images/4372317-949249de77f94715.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 单车骑行次数以5次以下居多

单车的使用频率越高共享的效果越好。从摩拜单车的数据看，在流动的单车中，5次以下占据了60%左右的出行。但1次、2次的也占据了30%左右的份额，说明摩拜单车的利用率也不是很高。

![单车骑行次数](http://upload-images.jianshu.io/upload_images/4372317-b63819b5686de984.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![单车骑行次数](http://upload-images.jianshu.io/upload_images/4372317-c2b03aee9fa7ddca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 从单车看城市发展 
从摩拜单车的热图分布来看，成都已经逐步呈现“双核”发展的态势，城市的新中心天府新区正在聚集更多的人和机会。


![双核发展](http://upload-images.jianshu.io/upload_images/4372317-900aca43266cc17d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

原来的老城区占有大量的单车，在老城区，热图显示在东城区占有更多的单车，可能和这里的商业（春熙路、太古里、万达）及人口密集的小区有直接的联系。 

![老城区](http://upload-images.jianshu.io/upload_images/4372317-2c5cc0f5d8bc3ada.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而在成都的南部天府新区越来越多也茁壮的发展起来，商业区域和住宅区域区分明显。在晚上，大量的单车聚集在华阳、世纪城、中和，而在上班时间，则大量聚集在软件园附近。

![软件园夜间](http://upload-images.jianshu.io/upload_images/4372317-529e0b586124777b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![软件园白天](http://upload-images.jianshu.io/upload_images/4372317-330ae930b2fc2934.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 在线网站

如果你对数据感兴趣，我已经创建了一个网站供大家使用，请用电脑访问：http://www.april1985.com/mobike/
（为节省开支，后端服务器已经关闭）