title: 写个暖宝宝给MAC用
date: 2015-02-08 16:57:15
tags:
- mac
- python
---

冬天好冷，加上mac的金属外壳，更冷。写个暖宝宝给MAC吧，好歹手会不会再冷了。

```python
import multiprocessing
import hashlib

def thread_func():
    while True:
        pass

if __name__ == "__main__":
    cpus = 4
    t = []
    for i in range(0,cpus):
        thread = multiprocessing.Process(target = thread_func)
        thread.start()
        print i
        t.append(thread)

    for i in range(0,cpus):
        t[i].join()
```

当然有这个玩意儿还不够，虽然把CPU占用率提上去了，但是MAC的风扇会狂转，这样可会觉得比较吵。下载个Macs Fan Control，将风扇的控制策略改为固定最低，这样就好了。

如果嫌火力不够大，将上面的cpus改为更大的值即可，但是这样会让系统反应很慢哟。

TIPS：现代的CPU当温度过高的时候会自动的降低频率，所以在高温时运算速度会下降的。

![](/images/2015/02/mac_fan_control.png)
![](/images/2015/02/cpu.png)