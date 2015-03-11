title: 淘宝旅行API分析
date: 2015-03-11 17:25:15
tags: 
- 爬虫
- crawler
- API
- 淘宝旅行
- 逆向工程
---

这一篇博客中我分析一下如何分析得到淘宝旅行的API，如何得到实时的机票信息。

我们先看看阿里旅行的[机票页面](http://trip.taobao.com/jipiao/)。打开Chrome Developer Tools发现要得到其中的信息感觉颇为复杂，很难去下手，一般我不会从主站下手。主站的各种防御做得比较好，往往页面结构复杂。其实如果我们换个思路，直接访问手机的站点，会简单很多。这是因为为手机做的站点其性能比较重要，往往做得比较简单，而且容易暴露一些API接口。

点击Chrome Developer

![Screen Shot 2015 03 11 At 16.30.17](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.30.17.png)

试着查一下深圳到成都的机票，看看会返回些什么。忽略掉图片等，发现h5apiUpdate.do这个call有意思

![我注意到一个jsonp的call](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.33.53.png)

将这个URL放到浏览器上面试一下，果断返回了所有的信息。
![Screen Shot 2015 03 11 At 16.35.13](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.35.13.png)

剩下的事情就简单很多了，一个GET请求就可以得到所有的信息。让我们来分析一下URL里面具体是怎么请求的。

URL请求，不要直接点击，点击后是请求失败的。

http://api.m.taobao.com/rest/h5ApiUpdate.do?callback=mtopjsonp1&type=originaljsonp&api=mtop.trip.flight.flightSuperSearch&v=1.0&data=%7B%22searchType%22%3A%221%22%2C%22depCityCode%22%3A%22SZX%22%2C%22arrCityCode%22%3A%22CTU%22%2C%22leaveDate%22%3A%222015-03-12%22%2C%22backDate%22%3A%222015-03-12%22%2C%22itineraryFilter%22%3A%220%22%2C%22sellerIds%22%3A%22%22%2C%22leaveFlightNo%22%3A%22%22%2C%22leaveCabinClass%22%3A%220%22%2C%22backCabinClass%22%3A%220%22%2C%22utdid%22%3A%22%22%2C%22depDate%22%3A%222015-03-12%22%7D&useNative=true&ttid=201300@travel_h5_3.1.0&appKey=12574478&t=1426062775998&sign=3feb52aed67967a2c47aa7a2b9f2a417

首先我们精简一下这个请求，更少的参数更能让我们专注，我最终去掉了callback和type，请求成功。但注意这个请求和之前的请求有一些区别，这个请求不是jsonp的，如果不是从浏览器直接访问，没有跨域请求的时候是可以不用jsonp的。

![Screen Shot 2015 03 11 At 16.40.01](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.40.01.png)

剩下事情就有点复杂了。

* api=mtop.trip.flight.flightSuperSearch&v=1.0&
这一串是版本号及API调用的接口，一般不用改变

* data=%7B%22searchType%22%3A%221%22%2C%22depCityCode%22%3A%22SZX%22%2C%22arrCityCode%22%3A%22CTU%22%2C%22leaveDate%22%3A%222015-03-12%22%2C%22backDate%22%3A%222015-03-12%22%2C%22itineraryFilter%22%3A%220%22%2C%22sellerIds%22%3A%22%22%2C%22leaveFlightNo%22%3A%22%22%2C%22leaveCabinClass%22%3A%220%22%2C%22backCabinClass%22%3A%220%22%2C%22utdid%22%3A%22%22%2C%22depDate%22%3A%222015-03-12%22%7D
包含了一些数据，但已经被quote过，基本上是一个json类一样，里面包含一些请求的数据。decode之后长这样：
![Screen Shot 2015 03 11 At 16.43.51](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.43.51.png)

* useNative=true&ttid=201300@travel_h5_3.1.0&appKey=12574478
这些固定字符串，暂时不管了

* t=1426062775998&sign=3feb52aed67967a2c47aa7a2b9f2a417
这两个参数，在不断请求的过程中是变化的，这个值一旦不对应，则API请求是失败的。这对我们的造成了很大的困扰。如果不清楚这里的算法，你不能算出sign，API请求完全无力。遇到这种情况，只有从javascript下手了。

是什么地方调用了这个API呢？我采用搜索url的方式。找h5ApiUpdate这个关键字，看有没有什么线索。最后在这个文件中发现了这个关键字。

![Screen Shot 2015 03 11 At 16.48.59](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.48.59.png)

糟糕的是这个代码已经被压缩过，你可能知道一些[站点](http://tool.lu/js/)，能够将javascript反混淆吧。但你不能直接利用这个代码做事情。我之前考虑是否做个proxy的站点，类似fiddler这种工具将这个压缩过的js给劫持了，然后返回我解压过的。想想整个过程都很复杂。

然后我去找了一下chrome的插件和stackoverflow。庆幸的是，chrome已经内置了一个非常牛逼的工具，且看这里。这一个简简单单的{}符号简直帮了大忙。

![Screen Shot 2015 03 11 At 16.52.13](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.52.13.png)

你看，pretty print后的代码，简直就是开卷考试了一样。
![Screen Shot 2015 03 11 At 16.52.59](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.52.59.png)

更为强大的是，用这个代码我居然可以打断点！
好吧，那我看看sign是怎么得到的。简单的搜索一下就可以找到了：
![Screen Shot 2015 03 11 At 16.55.17](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.55.17.png)

看到了没有，所有的秘密都在这里。
![Screen Shot 2015 03 11 At 16.57.20](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.57.20.png)

这些值和cookie相关，做一次请求，打几个断点，所有的秘密都清楚了。
![Screen Shot 2015 03 11 At 16.59.22](/images/2015/03/Screen%20Shot%202015-03-11%20at%2016.59.22.png)

好吧，我们来写一个简单的程序来模拟吧，请参考：https://github.com/derekhe/alitripAPI

nodejs可以非常好的完成任务，而且阿里不会阻止你采集（这点很大方啊）。我后来想把它写到手机上，用cordova做个app的，但可惜cordova不支持cookie这事情，让整个事情搁浅了。后续试试原生的http request，应该可以解决问题。