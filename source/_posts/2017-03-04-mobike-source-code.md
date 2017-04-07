title: 摩拜单车爬虫源码及解析
date: 2017-03-04
tags: 
- 摩拜
- 爬虫
- 大数据
---

前两篇文章分析了我为什么抓取摩拜单车的接口以及数据分析的结果，这篇文章中讲直接提供可运行的源代码供学习。

> 声明：
> 此爬虫仅用于学习、研究用途，请不要用于非法用途。任何由此引发的法律纠纷自行负责。

没耐心看文章的请后直接：
```
git clone https://github.com/derekhe/mobike-crawler
python3 crawler.py
```
爽了以后请别忘了给个star和打赏！

# 目录结构
* \analysis - jupyter做数据分析
* \influx-importer - 导入到influxdb，但之前没怎么弄好
* \modules - 代理模块
* \web - 实时图形化显示模块，当时只是为了学一下react而已，效果请见[这里](www.april1985.com/mobike)
* crawler.py - 爬虫核心代码
* importToDb.py - 导入到postgres数据库中进行分析
* sql.sql - 创建表的sql
* start.sh -　持续运行的脚本

# 思路

核心代码放在crawler.py中，数据首先存储在sqlite3数据库中，然后去重复后导出到csv文件中以节约空间。

摩拜单车的API返回的是一个正方形区域中的单车，我只要按照一块一块的区域移动就能抓取到整个大区域的数据。

left,top,right,bottom定义了抓取的范围，目前是成都市绕城高速之内以及南至南湖的正方形区域。offset定义了抓取的间隔，现在以0.002为基准，在DigitalOcean 5$的服务器上能够15分钟内抓取一次。

```
    def start(self):
        left = 30.7828453209
        top = 103.9213455517
        right = 30.4781772402
        bottom = 104.2178123382

        offset = 0.002

        if os.path.isfile(self.db_name):
            os.remove(self.db_name)

        try:
            with sqlite3.connect(self.db_name) as c:
                c.execute('''CREATE TABLE mobike
                    (Time DATETIME, bikeIds VARCHAR(12), bikeType TINYINT,distId INTEGER,distNum TINYINT, type TINYINT, x DOUBLE, y DOUBLE)''')
        except Exception as ex:
            pass

```

然后就启动了250个线程，至于你要问我为什么没有用协程，哼哼～～我当时没学～～～其实是可以的，说不定效率更高。

由于抓取后需要对数据进行去重，以便消除小正方形区域之间重复的部分，最后的group_data正是做这个事情。

```
        executor = ThreadPoolExecutor(max_workers=250)
        print("Start")
        self.total = 0
        lat_range = np.arange(left, right, -offset)
        for lat in lat_range:
            lon_range = np.arange(top, bottom, offset)
            for lon in lon_range:
                self.total += 1
                executor.submit(self.get_nearby_bikes, (lat, lon))

        executor.shutdown()
        self.group_data()
```

最核心的API代码在这里。小程序的API接口，搞几个变量就可以了，十分简单。

```
    def get_nearby_bikes(self, args):
        try:
            url = "https://mwx.mobike.com/mobike-api/rent/nearbyBikesInfo.do"

            payload = "latitude=%s&longitude=%s&errMsg=getMapCenterLocation" % (args[0], args[1])

            headers = {
                'charset': "utf-8",
                'platform': "4",
                "referer":"https://servicewechat.com/wx40f112341ae33edb/1/",
                'content-type': "application/x-www-form-urlencoded",
                'user-agent': "MicroMessenger/6.5.4.1000 NetType/WIFI Language/zh_CN",
                'host': "mwx.mobike.com",
                'connection': "Keep-Alive",
                'accept-encoding': "gzip",
                'cache-control': "no-cache"
            }

            self.request(headers, payload, args, url)
        except Exception as ex:
            print(ex)
```

最后你可能要问频繁的抓取IP没有被封么？其实摩拜单车是有IP的访问速度限制的，只不过破解之道非常简单，就是用大量的代理。

我是有一个代理池，每天基本上有8000以上的代理。在ProxyProvider中直接获取到这个代理池然后提供一个pick函数用于随机选取得分前50的代理。请注意，我的代理池是每小时更新的，但是代码中提供的jsonblob的代理列表仅仅是一个样例，过段时间后应该大部分都作废了。

在这里用到一个代理得分的机制。我并不是直接随机选择代理，而是将代理按照得分高低进行排序。每一次成功的请求将加分，而出错的请求将减分。这样一会儿就能选出速度、质量最佳的代理。如果有需要还可以存下来下次继续用。

```
class ProxyProvider:
    def __init__(self, min_proxies=200):
        self._bad_proxies = {}
        self._minProxies = min_proxies
        self.lock = threading.RLock()

        self.get_list()

    def get_list(self):
        logger.debug("Getting proxy list")
        r = requests.get("https://jsonblob.com/31bf2dc8-00e6-11e7-a0ba-e39b7fdbe78b", timeout=10)
        proxies = ujson.decode(r.text)
        logger.debug("Got %s proxies", len(proxies))
        self._proxies = list(map(lambda p: Proxy(p), proxies))

    def pick(self):
        with self.lock:
            self._proxies.sort(key = lambda p: p.score, reverse=True)
            proxy_len = len(self._proxies)
            max_range = 50 if proxy_len > 50 else proxy_len
            proxy = self._proxies[random.randrange(1, max_range)]
            proxy.used()

            return proxy
```

在实际使用中，通过proxyProvider.pick()选择代理，然后使用。如果代理出现任何问题，则直接用proxy.fatal_error()降低评分，这样后续就不会选择到这个代理了。

```
    def request(self, headers, payload, args, url):
        while True:
            proxy = self.proxyProvider.pick()
            try:
                response = requests.request(
                    "POST", url, data=payload, headers=headers,
                    proxies={"https": proxy.url},
                    timeout=5,verify=False
                )

                with self.lock:
                    with sqlite3.connect(self.db_name) as c:
                        try:
                            print(response.text)
                            decoded = ujson.decode(response.text)['object']
                            self.done += 1
                            for x in decoded:
                                c.execute("INSERT INTO mobike VALUES (%d,'%s',%d,%d,%s,%s,%f,%f)" % (
                                    int(time.time()) * 1000, x['bikeIds'], int(x['biketype']), int(x['distId']),
                                    x['distNum'], x['type'], x['distX'],
                                    x['distY']))

                            timespend = datetime.datetime.now() - self.start_time
                            percent = self.done / self.total
                            total = timespend / percent
                            print(args, self.done, percent * 100, self.done / timespend.total_seconds() * 60, total,
                                  total - timespend)
                        except Exception as ex:
                            print(ex)
                    break
            except Exception as ex:
                proxy.fatal_error()
```

好了，基本上就到此了～～～其他的代码自己研究吧～～～