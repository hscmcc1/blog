title: 2017自由职业大数据分析
date: 2017-05-24
tags: 
- Freelancer
- 自由职业
- 大数据
---

# 阅读须知
> 本文以Freelancer.com的公开项目及用户数据，对自由职业进行大数据分析。由于Freelancer.com代表线上的自由职业，并不代表所有的自由职业划分，请勿以本文结论以偏概全。

> 本文仅作为自我学习、研究用途，并不对数据的完整性、正确性负责，也不对使用、引用本文造成的法律问题负责。

> 本文未经同意不允许任何平台转载。非法转载将追究其责任。

# 简介
Freelancer.com成立于2009年，后收购了数家自由职业者公司。成为世界上自由职业者相关网站的领头羊，分析该网站的数据能够窥见自由职业的现状和发展趋势。

> 【重点推荐】以下分析均在[在线互动版本](http://www.april1985.com/freelancer)中包含，请在电脑上点击查看，图片更清晰，可互动。

# 数据来源

该报告数据来源于[Freelancer.com的公开API](https://www.freelancer.com/api/docs/docs.html)。采集了从2000年1月到2017年3月的上千万的数据包含项目、用户、投标、用户评价数据。存储为易于处理的json格式，整体打包大小约为40G。总计花费大约20天左右。

# 采集数据和网站数据对比

从采集到的数据来看，则项目数量在740万左右，用户数量在1700万，两者相差大约56%和35%左右。项目数和用户数总体上都存在一些差异，差异可能来自于不同的统计方式或者网站删除了部分违规的项目。

![](http://upload-images.jianshu.io/upload_images/4372317-f92d5df1c084417d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 项目和用户增长情况
Freelaner的用户规模和项目规模在加速增长中，预计在2018年初突破2000万用户和800万项目。

![](http://upload-images.jianshu.io/upload_images/4372317-8c93322b3396608b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 会员分布

在Freelancer的会员体系中，免费的会员有1727万名，占据了99.66%（图中无标示）。付费会员总共有5.8万，免费和付费会员比为295:1。

> 注：其中有一些会员类型可能是Freelancer之前的老会员类型。

![](http://upload-images.jianshu.io/upload_images/4372317-d55ed51821d03b63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 自由职业者全球分布

全球分布中，以美国和印度为第一梯队，菲律宾、印度尼西亚、巴基斯坦紧为第二梯队，英国、澳大利亚、孟加拉国紧跟其后，巴西等国家也有较大的比重。

![](http://upload-images.jianshu.io/upload_images/4372317-e6fdaf11e7f0c5af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 项目所有者全球分布

和自由职业者分布类似，美国和印度的项目占领了很大的份额，英国、澳大利亚、加拿大等发达国家保持着稳定的市场，菲律宾、巴基斯坦跟随其后，这些项目很有可能是二次外包的项目。

![](http://upload-images.jianshu.io/upload_images/4372317-69067935dedf4d03.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 业务种类分布

网站35%的项目都是网站建设和软件相关业务，有23%的项目是设计类的项目，10%的是写作和内容类项目。这三类项目占据了7成以上的份额。


![](http://upload-images.jianshu.io/upload_images/4372317-9ddce0804fe2515e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 具体业务分布

在具体的业务分布中，前60%的市场都和网站建设和设计有关。其中PHP占据了20%的市场，这和WordPress等基于PHP的CMS网站建设有很大关系。Graphic Design占据12%的市场，紧接着又是网站建设。其他的类型则市场相对较小。

![](http://upload-images.jianshu.io/upload_images/4372317-3a53cd2b963af1ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 价值迁移图

此图展示了项目价值的流动方向，可以看欧美、澳洲有项目和资金的大量流出。印度、巴基斯坦作为外包大国，有非常多的项目和资金流入。全球外包的中心图中可见一斑。

![](http://upload-images.jianshu.io/upload_images/4372317-33f9c434557e84ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 全球接单数TOP20

不出所料，全球接单数最多的20名自由职业中，来自于印度、巴基斯坦、菲律宾等欠发达国家的最多。 但接单数多并不意味着赚取的钱最多，接单数最多的平均每单才1美元不到，而最高的也才25美元， 说明在这些国家接的单都是价格低廉的单，总体赚钱不多。

![](http://upload-images.jianshu.io/upload_images/4372317-9c68f9997144a6ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# TOP 20接单种类分析

图标、媒体设计类是最多的单，其中Graphic Design和Logo Design占据了最多的份额。其次是网站建设，在网站建设中PHP依然是很火的，和WordPress等PHP架构的网站息息相关。

![](http://upload-images.jianshu.io/upload_images/4372317-1674736ffb2d258f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 中国接单数TOP20
虽然在全世界范围内中国的自由职业者不算太多，但对比全球接单最多的国家（印度、巴基斯坦等）
中国的自由职业者平均每单的价格均超过了15美元，这应该和中国的实际消费水平相关。

与全球类似，第一名SunrisePHP为了和其他国家的低价竞争，只能以低价取胜，虽然数量很多但整体收入欠佳。
形成鲜明对比的zhengnami13，平均每单价格高的700美元，与选择的高质量的工程的性质有关。

![](http://upload-images.jianshu.io/upload_images/4372317-7b18e6d17d78327a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 用户ZHENGNAMI13分析
## 收入分析
从[自我介绍](https://www.freelancer.com/u/zhengnami13.html)上可以看到，该用户来自于中国的沈阳。 从2013年4月加入，Rate为50$每小时，擅长移动开发。 该用户最低每单都在860$以上，每月最低都有2000$以上的收入，几乎每月都有3个以上的单，接单质量相对较高。


![](http://upload-images.jianshu.io/upload_images/4372317-169762a06300d584.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 接单类型和客户分析

该用户主攻方向和描述一致，主要是涉及到移动领域的开发。具体来讲iOS的比Android开发更多，纯HTML5的开发也占有少量的比例。 该用户的客户主要是欧美和澳洲的客户，这些客户质量要求高、资金雄厚，做好这些客户也可以给自己打下良好的口碑的基础。

![](http://upload-images.jianshu.io/upload_images/4372317-8258316bd90c13f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 总结

Freelancer.com的数据很有意思，具有15年以上的历史数据并且数据大部分可见。在分析这些数据的过程中可以发现很多有意思的点，由于篇幅所限在这个报告中不能一一呈现。如果您对这个报告以及分析感兴趣，希望从数据中发现更多有意义的洞见，请联系我。