---
author: hesicong
comments: true
date: 2009-08-26 01:52:17+00:00
layout: post
slug: openjtag-eclipse-3-5-gdb-mini2440-photo-tutorial
title: OpenJTAG+Eclipse 3.5+GDB+Mini2440图文教程
wordpress_id: 1959
categories:
- 硬件
tags:
- eclipse
- gdb
- mini2440
- OpenJTAG
---

最近学看了些书，对嵌入式有进一步了解了。开发昨天花了180大洋买了个OpenJTAG调试器，以便跟踪调试程序，查看寄存器的变化，进一步了解ARM9的运作原理。 OpenJTAG买回来了折腾了好久终于可以用了。

首先是操作系统的问题。我认为理想的开发环境是Linux+Eclipse来开发。在Windows里面只能用虚拟机安装。我再 VirtualBox 3.0.4里面装好了Ubuntu和VboxAdditionTools，但是始终无法将Host的USB JTAG设备分配过去，显示previous request is busy。晕，直接用不了。尝试用Vmware 6.5，装的Ubuntu 8.10竟然不能用Vmware Tools，原因是和内核不兼容。好嘛，那我只有装Native Ubuntu了。为了便于管理，直接用Wubi装了个9.04，升级到最新的软件后，开始了OpenJTAG之旅。

将OpenJTAG插入后，会多出来一个USB设备，在/dev/ttyUSB0。说明连接正常。

我的开发板拨到NAND档的，里面有一个2440test程序，会在一开机就启动，会设置MMU、Cache等。这点对于后来的JTAG调试造成了一些麻烦，要比说明书多一些步骤才能正确运行调试。

* * *

**首先我们来看看怎么用手动方式调试：**

将光盘附带的friendly-arm/leds复制到工作目录/home/derekhe/workspace/leds，然后再命令行中输入 make编译程序，得到leds_elf文件和leds.bin文件。连接好OpenJTAG和开发板，打开电源，然后插上OpenJTAG。在终端中运行

``` bash
derekhe@ubuntu:~/workspace$ openocd -f ~/workspace/openocd.cfg
```

注意将-f 后面修改为你openocd.cfg所在真实路径。如果此文件在当前目录下(如本例)，可以直接运行openocd。

我的openocd.cfg如下：

```
telnet_port 4444
gdb_port 3333
interface ft2232
jtag_speed 0
ft2232_vid_pid 0x1457 0x5118
ft2232_layout "jtagkey_prototype_v1"
reset_config trst_and_srst
jtag_device 4 0x1 0xf 0xe
daemon_startup attach
target arm920t little reset_run 0 arm920t
arm7_9 fast_memory_access enable
working_area 0 0x200000 0x4000 backup
#flash bank cfi 0 0x100000 2 2 0
#debug_level 3
nand device s3c2440 0
run_and_halt_time 0 5000
ft2232_device_desc "USB<=>JTAG&RS232"
```

此时会显示：

```
Info:    options.c:50 configuration_output_handler(): jtag_speed: 0, 0
Info:    options.c:50 configuration_output_handler(): Open On-Chip Debugger 1.0 (2008-10-04-09:26) svn:717
Info:    options.c:50 configuration_output_handler(): fast memory access is enabled
Info:    jtag.c:1389 jtag_examine_chain(): JTAG device found: 0x0032409d (Manufacturer: 0x04e, Part: 0x0324, Version: 0x0)
```

说明jtag已经找到了，可以开始调试了。 下一步开启另外一个终端，开始运行telnet程序，链接OpenJTAG服务器

```
derekhe@ubuntu:~/workspace$ telnet localhost 4444
```

终端输出：

```
derekhe@ubuntu:~$ telnet localhost 4444
Trying ::1...
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
Open On-Chip Debugger
>
```

由于上电以后NAND的程序会自动运行，所以必须先使用halt命令暂停：

```
> halt
target state: halted
target halted in ARM state due to debug request, current mode: Supervisor
cpsr: 0x60000013 pc: 0x30001150
MMU: enabled, D-Cache: enabled, I-Cache: enabled
>
```

此时可以看到，MMU和Cache处于enable状态。在下载程序之前要清除这两个状态才行。运行arm920t cp15 2 0和step指令，可以将MMU和Cache清除：

```
> arm920t cp15 2 0
2: 00000000
> step
target state: halted
target halted in ARM state due to single step, current mode: Supervisor
cpsr: 0x60000013 pc: 0x30001154
MMU: disabled, D-Cache: disabled, I-Cache: disabled
>
```

然后载入image(似乎只有用绝对路径才行)：

```
> load_image /home/derekhe/workspace/leds/leds_elf
172 byte written at address 0x00000000
downloaded 172 byte in 0.007424s
>
```

然后开始运行：

```
> resume 0x0
```

程序就开始运行，可见开发板上灯不停闪烁。

* * *

**好，了解整个过程后，我们再来看看OpenJTAG怎么和GDB一起使用：**

OpenOCD的GDB服务端在3333端口，可以在openocd.cfg里面配置。

重启开发板。在OpenOCD服务端运行的情况下，打开另外一个终端。在shell中输入：

```
derekhe@ubuntu:~$ arm-linux-gdb
```

我使用的arm-linux-gdb是友善提供的交叉编译器里面的，版本比较新：

```
GNU gdb (Sourcery G++ Lite 2008q3-72) 6.8.50.20080821-cvs
Copyright (C) 2008 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "--host=i686-pc-linux-gnu --target=arm-none-linux-gnueabi".
For bug reporting instructions, please see:
.
(gdb)
```

首先链接OpenOCD，输入：target remote localhost:3333

```
(gdb) target remote localhost:3333
Remote debugging using localhost:3333
warning: while parsing target memory map (at line 2): Required element  is missing
0x00000000 in ?? ()
(gdb)
```

此时已经连接上远程调试服务器。我们来看看目前的状态，使用monitor poll来运行OpenOCD状态显示命令。

```
(gdb) monitor poll
target state: halted
target halted in ARM state due to debug request, current mode: Supervisor
cpsr: 0x60000013 pc: 0x30001150
MMU: enabled, D-Cache: enabled, I-Cache: enabled
(gdb)
```

此时可以看出MMU和Cache已经开启。下载程序前先停止程序并关闭MMU和Cache：

```
(gdb) monitor halt
(gdb) monitor arm920t cp15 2 0
2: 00000000
(gdb) monitor step
(gdb) monitor poll
target state: halted
target halted in ARM state due to single step, current mode: Supervisor
cpsr: 0x60000013 pc: 0x30001154
MMU: disabled, D-Cache: disabled, I-Cache: disabled
```

此时MMU和Cache已经关闭。下面我们载入文件leds_elf文件：

```
(gdb) file ~/workspace/leds/leds_elf
A program is being debugged already.
Are you sure you want to change the file? (y or n) y
Reading symbols from /home/derekhe/workspace/leds/leds_elf...done.
(gdb)
```

打开软件中断：

```
 (gdb) monitor arm7_9 sw_bkpts enable
 software breakpoints enabled
 (gdb)
```

将文件载入内存：

```
(gdb) load
Loading section .text, size 0xac lma 0x0
Start address 0x0, load size 172
Transfer rate: 20 KB/sec, 172 bytes/write.
```

重新从0x0开始运行：

```
(gdb) monitor resume 0x0
```

此时会看到灯在闪烁了。

* * *

**下面来看看如何在eclipse 3.5里面进行配置并调试**

首先在工作目录~/workspace内创建一个gdbinit文件(名字可以随便取，这里为了以后好对应):

```
target remote localhost:3333
monitor halt
monitor arm920t cp15 2 0
monitor step
monitor arm7_9 sw_bkpts enable
load
monitor soft_reset_halt
```

打开eclipse，File->New->C Project，将工程取名为leds

[](/images/eclipse-arm-linux/screenshot-c-project.png)![](/images/eclipse-arm-linux/image/thumb/screenshot-c-project.png)

此时eclipse workspace没有任何文件：

[](/images/eclipse-arm-linux/screenshot-c-c-eclipse.png)![](/images/eclipse-arm-linux/image/thumb/screenshot-c-c-eclipse.png)

将OpenJTAG附送的光盘里面的Ubuntu/examples/friendly-arm/leds内的所有文件拷贝到工作目录

[](/images/eclipse-arm-linux/screenshot-c-c-eclipse-1.png)![](/images/eclipse-arm-linux/image/thumb/screenshot-c-c-eclipse-1.png)

此时按ctrl+B即可生成文件。

下面配置Debug相关参数：Run->Debug Configuration

[](/images/eclipse-arm-linux/screenshot-debug-configurations.png)![](/images/eclipse-arm-linux/image/thumb/screenshot-debug-configurations.png)

双击C/C++ Application，会生成默认的leds default配置：

[](/images/eclipse-arm-linux/screenshot-debug-configurations-1.png)![](/images/eclipse-arm-linux/image/thumb/screenshot-debug-configurations-1.png)

再C/C++ Application中填入生成的可执行文件：/home/derekhe/workspace/leds/leds_elf

[](/images/eclipse-arm-linux/screenshot-debug-configurations-5.png)![](/images/eclipse-arm-linux/image/thumb/screenshot-debug-configurations-5.png)

选择Debugger，在新页中选择Debugger：gdbserver Debugger

调整gdb调整为arm-linux-gdb，GDB command file为~/workspace/gdbinit

[](/images/eclipse-arm-linux/screenshot-debug-configurations-3.png)![](/images/eclipse-arm-linux/image/thumb/screenshot-debug-configurations-3.png)

选择connection，将type选择为TCP, host name or IP Address为localhost，port number为3333

[](/images/eclipse-arm-linux/screenshot-debug-configurations-4.png)![](/images/eclipse-arm-linux/image/thumb/screenshot-debug-configurations-4.png)

在按debug按钮之前，请现在终端执行openocd进入等待模式。然后就可以按debug，此时会进入调试状态了。

[](/images/eclipse-arm-linux/screenshot-debug-leds-leds-c-eclipse_0.png)![](/images/eclipse-arm-linux/image/thumb/screenshot-debug-leds-leds-c-eclipse_0.png)

剩下的功能就和eclipse的基本使用一致了，这里不再阐述。

另外我发现一些问题，restart按钮不能用，提示：

> Exception(s) occurred attempting to restart.
> Target request failed: The "remote" target does not support "run".  Try "help target" or "continue".
> The "remote" target does not support "run".  Try "help target" or "continue".

还不知道怎么配置eclipse使用另外的命令。
