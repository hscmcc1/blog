title: Android SDK Offline Download 
date: 2015-07-20
tags: 
- android
- sdk
---
[Github Repo](https://github.com/derekhe/android-sdk-offline)

Updated:
> 做出这个工具后，我司达人告诉我，只需要把SDK Manager代理服务器设置为www.google.cn或者其国内IP即可，速度可以达到满格。在电信网络中测试，果然很霸道，请大家尝试。

由于Google被GFW的问题Android SDK下载速度总是非常的慢，甚至用了[代理服务器](http://www.androiddevtools.cn/)速度也很慢，因为是单线程的。我写了这个工具，可以在线生成需要的SDK的下载链接，这样就可以方便的用你喜欢的工具下载了（迅雷和QQ旋风速度都很不错）。
如果还不能下载，则可以在下载工具中设置代理服务器，让其进行多线程下载，速度也非常理想。

<!--more-->

This tool help to you generate latest offline down links for Android SDK. This is helpful when you don't have internet access or speed is low.

You can use your favorite download tool to download them. Then put these downloaded files into sdk/temp folder and open Android SDK Manager. Click the items in Android SDK Manager, it will read from your downloaded cache.

If your can't get the Android SDK Manager items or if they are not update, or the download speed is slow, please try to use the [proxy listed here](http://www.androiddevtools.cn/)

# Online site 在线网址
http://www.april1985.com/android-sdk-offline/

# Clone and run locally

```
git clone https://github.com/derekhe/android-sdk-offline.git
npm install
cd web
bower install
cd ..
npm install -g http-server
//You may need to set proxy if your network is blocked
npm run generate
npm run serve
```

open your browser and navigate to localhost:8080