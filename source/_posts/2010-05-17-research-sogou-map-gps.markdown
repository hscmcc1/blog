---
author: hesicong
comments: true
date: 2010-05-17 05:35:35+00:00
layout: post
slug: research-sogou-map-gps
title: 搜狗地图研究-坐标系对应
wordpress_id: 2352
categories:
- 软件
---

搜狗地图是搜狐搜狗了以前有名的go2map后重新制作的地图，目前google地图已经很长时间没有任何更新，已经没有太大利用价值。而国内的搜狗地图和百度地图在地图上的开发已经很完善，地图数据、POI数据等都是比较新的，具有较大的利用价值。例如你可以把这些地图下载下来，然后当做电子地图来使用。但搜狗和百度地图的劣势在于没有手机客户端，而且各自采用了其自定义的坐标系统，即便离线使用，也无法使用GPS进行导航。使用搜狗地图即便是小范围匹配后，大范围的应用也会发现有较为严重的偏移。所以研究GPS坐标系和地图上的点的对应关系就成了做离线地图浏览的最关键的部分。

打开搜狗地图后，地址栏显示#c后面的就是其坐标和当前的层：

例如北京的URL是：http://map.sogou.com/i.html#c=12956000,4824875,10

12956000代表了自定义的经度坐标，4824875代表了自定义的纬度坐标，10是第10级。坐标是对应于屏幕正中央的位置的坐标。

如果直接修改url然后敲回车，地图是没有任何反应的，重新刷新后更新的位置就会显示出来。拖动地图，URL会进行刷新。

[](/images/sogoumap/capture.png)![](/images/sogoumap/image/thumb/capture.png)

那么如何得到GPS的位置和坐标系的对应关系呢？

这时候就要用上Google Earth和搜狗的卫星地图功能了。

切换到搜狗的卫星地图发现，搜狗的地图和卫星地图的融合相当好，也就是说没有使用常见的偏移的手段对电子地图进行处理。这样事情就好办了。只需要在Google Earth和搜狗找到相同的位置，就可以知道GPS位置和对应的搜狗的坐标了。

我们以人民英雄纪念碑为例(为什么选纪念碑呢，这个自己想象，哈哈)，说明这个对应法：

首先我们找到英雄纪念碑，将其置于搜狗的正中央：

[](/images/sogoumap/capture2.jpg)![](/images/sogoumap/image/thumb/capture2.jpg)

这时地址栏显示：

http://map.sogou.com/i.html#c=12956777.34375,4824206.0546875,18&lq=%u5929%u5B89%u95E8&where=北京&page=1&hb=1,1

关键看这个：12956777.34375,4824206.0546875

好了，打开Google Earth。在开始之前，我们先把Google Earth的坐标系设置一下：工具->选项->显示经度/纬度->小数度数，这样在Google Earth下显示的就是对应的小数坐标了。

[](/images/sogoumap/capture3.jpg)![](/images/sogoumap/image/thumb/capture3.jpg)

可见坐标是39.905588，116.391304，这是实际的GPS位置。

好了，这就有了一组数据了：

纬度：39.905588->4824206.0546875

精度：116.391304->12956777.34375

你多找几组这样的数据，就会发现，这些数据中有一些规律了。而且在小范围内基本上是线性关系，两点之间的线性插值也可以得到目标位置的GPS位置对应的值。但别着急，这肯定不是线性关系，要不然我也不会写这个了。呵呵。

如果这样做下去，岂不是要把人累死，而且速度还不是那么的快。如何短时期内得到大量的数据呢？

最初我的想法是用图上的标记点来做，但搜狗地图的js是加密过的，很难调试。山穷水尽之后，无意中发现了搜狗地图的路书功能，它能够将GPS的轨迹给你在地图上表示出来。这不是正是我想要的么？

好吧，我就上传一个最简单的gpx上去，该gpx只包括了两个点(30，73)和(35，73)：

``` xml
<gpx xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.topografix.com/GPX/1/1" version="1.1" xsi:schemalocation="http://www.topografix.com/GPX/1/1 http://www.topografix.com/GPX/1/1/gpx.xsd" creator="KML2GPX">
<metadata>
<name>test</name>
<desc>test</desc>
<link href="http://www.garmin.com">
<text>Garmin International</text>
</link>
</metadata>
<trk>
<name>Holux2010/03/12_19:23</name>
<trkseg>
<trkpt lat="30" lon="73">
<ele>0</ele>
</trkpt>
<trkpt lat="35" lon="73">
<ele>0</ele>
</trkpt>
</trkseg>
</trk>
<extensions>
</extensions>
</gpx>
```

至于gpx文件的结构，各位还是自己网上找一找。
好了，让我们来制作路书。上传的同时请注意你的firebug的网络传输，注意upload这个。
[](/images/sogoumap/capture4.jpg)![](/images/sogoumap/image/thumb/capture4.jpg)
在gpx传输完成以后，upload会返回一系列的html，其中就包含了转换后的坐标系：

```
    <html>
    <head>
    <title>hidden frame</title>
    <script type="text/javascript">
    	function init() {

    		parent.loadupover("上传完毕！", "9a8080a728a457650128a4ada145057c","gpx轨迹", [{"id":"9a8080a728a457650128a4ada146057d","title":"轨迹段1","start":{"id":"9a8080a728a457650128a4ada146057e","title":"途经点1","description":null,"position":{"type":"POINT","x":8126411.250799975,"y":1678043.1192767194},"images":null},"end":{"id":"9a8080a728a457650128a4ada147057f","title":"途经点2","description":null,"position":{"type":"POINT","x":8126411.25084977,"y":7135255.941276442},"images":null},"timeSequence":"{}","length":4336600.054319387,"description":null,"segIdx":1,"path":{"type":"LINESTRING","minx":8126411.250799975,"miny":1678043.1192767194,"maxx":8126411.25084977,"maxy":7135255.941276442,"levels":"@@","points":"u{~nNulleB?wralI"},"navi":null,"subSegments":null},{"id":"9a8080a728a457650128a4ada1470580","title":"轨迹段2","start":{"id":"9a8080a728a457650128a4ada1480581","title":"途经点3","description":null,"position":{"type":"POINT","x":8126411.250799975,"y":1678043.1192767194},"images":null},"end":{"id":"9a8080a728a457650128a4ada1480582","title":"途经点4","description":null,"position":{"type":"POINT","x":8126411.25084977,"y":7135255.941276442},"images":null},"timeSequence":"{}","length":4336600.054319387,"description":null,"segIdx":2,"path":{"type":"LINESTRING","minx":8126411.250799975,"miny":1678043.1192767194,"maxx":8126411.25084977,"maxy":7135255.941276442,"levels":"@@","points":"u{~nNulleB?wralI"},"navi":null,"subSegments":null}], [])


    	}
    </script>
    </head>
    <body onload="init()">
    </body>
    </html>
```

注意POINT后面的x,y，那你就是搜狗的坐标，和你上传的坐标是一一对应的关系。
好了吧，事情说清楚了吧。发现这个还是费了很大的力气的啦。
当然，如果你的gpx文件太大了(你很贪心，每次都要上传很多个点)，那么firebug这个显示将不完全。
给你点点提示：根据路书展示时候的flash的地址访问，可以得到一个json文件。json文件里面包含了所有的坐标对应。至于具体地址怎么回事，我就不公布了。
好了，编写个程序，将gpx文件一个一个上传上去，然后再得到坐标。我将GPS的坐标以0.01的步进进行转换，得到了细致且大量的转换数据。
此后，就是用数学工具进行拟合计算了(这几年大学没白读)~
最后得到了从GPS转换到搜狗地图的公式：
[](/images/sogoumap/capture5.jpg)![](/images/sogoumap/image/thumb/capture5.jpg)
当然，如果你想得到这个公式，可不能不劳而获哦，联系我。或者你自己做吧，反正思路是给你说好了的。呵呵。
如果你想试试这个公式好不好用，请访问：[http://april1985.com/sogoumap/](http://april1985.com/sogoumap/)