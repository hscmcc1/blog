---
author: hesicong
comments: true
date: 2008-12-26 15:58:36+00:00
layout: post
slug: opencascade-6-3-0-compile-error-solution
title: OpenCASCADE 6.3.0编译出错的解决方法
wordpress_id: 1502
categories:
- 其他
tags:
- Linux
- OpenCASCADE
---

在Linux下编译OpenCASCADE 6.3.0，出现：

```
g++ -DHAVE_CONFIG_H -I. -I. -I../../.. -I../../../inc -I../../../drv/EDL -I../../../src/EDL -I../../../drv/MS -I../../../src/MS -I../../../drv/WOKAPI -I../../../src/WOKAPI -I../../../drv/WOKBuilder -I../../../src/WOKBuilder -I../../../drv/WOKDFLT -I../../../src/WOKDFLT -I../../../drv/WOKDeliv -I../../../src/WOKDeliv -I../../../drv/WOKMake -I../../../src/WOKMake -I../../../drv/WOKOBJS -I../../../src/WOKOBJS -I../../../drv/WOKOrbix -I../../../src/WOKOrbix -I../../../drv/WOKStep -I../../../src/WOKStep -I../../../drv/WOKTools -I../../../src/WOKTools -I../../../drv/WOKUnix -I../../../src/WOKUnix -I../../../drv/WOKUtils -I../../../src/WOKUtils -I../../../drv/WOKernel -I../../../src/WOKernel -DNDEBUG -DNo_Exception -O2 -g -m32 -march=i586 -mtune=i686 -fmessage-length=0 -D_FORTIFY_SOURCE=2 -ffriend-injection -fpermissive -DCSFDB -DOCC_CONVERT_SIGNALS -DLIN -DLININTEL -D_GNU_SOURCE=1 -O2 -MT WOKUnix_FDescr.lo -MD -MP -MF .deps/WOKUnix_FDescr.Tpo -c ../../../src/WOKUnix/WOKUnix_FDescr.cxx -fPIC -DPIC -o WOKUnix_FDescr.lo
../../../src/WOKUnix/WOKUnix_FDescr.cxx: In member function 'void WOKUnix_FDescr::Dup()':
../../../src/WOKUnix/WOKUnix_FDescr.cxx:249: warning: ignoring return value of 'int dup(int)', declared with attribute warn_unused_result
../../../src/WOKUnix/WOKUnix_FDescr.cxx: In member function 'Handle_TCollection_HAsciiString WOKUnix_FDescr::ReadLine()':
../../../src/WOKUnix/WOKUnix_FDescr.cxx:355: warning: ignoring return value of 'char* fgets(char*, int, FILE*)', declared with attribute warn_unused_result
../../../src/WOKUnix/WOKUnix_FDescr.cxx: In function 'FILE* _wokunix_fdopen(int)':
../../../src/WOKUnix/WOKUnix_FDescr.cxx:436: warning: deprecated conversion from string constant to 'char*'
../../../src/WOKUnix/WOKUnix_FDescr.cxx:443: warning: deprecated conversion from string constant to 'char*'
../../../src/WOKUnix/WOKUnix_FDescr.cxx:449: warning: deprecated conversion from string constant to 'char*'
In function 'int open(const char*, int, ...)',
inlined from 'WOKUnix_FDescr WOKUnix_FDescr::BuildNamedPipe()' at ../../../src/WOKUnix/WOKUnix_FDescr.cxx:205:
/usr/include/bits/fcntl2.h:51: error: call to '__open_missing_mode' declared with attribute error: open with O_CREAT in second argument needs 3 arguments
make[3]: *** [WOKUnix_FDescr.lo] Error 1
make[3]: Leaving directory `/home/anubis/src/rpm/BUILD/OpenCASCADE6.3.0/ros/adm/make/TKWOK'
make[2]: *** [all-recursive] Error 1
make[2]: Leaving directory `/home/anubis/src/rpm/BUILD/OpenCASCADE6.3.0/ros/adm/make'
make[1]: *** [all-recursive] Error 1
make[1]: Leaving directory `/home/anubis/src/rpm/BUILD/OpenCASCADE6.3.0/ros'
make: *** [all] Error 2</blockquote>
```

注意函数体BuildNamedPipe出错。在[论坛上找到解决方案](http://www.opencascade.org/org/forum/thread_14325/)

只需要修改ros/src/WOKUnix/WOKUnix_FDescr.cxx的205行为：

```
myFileChannel = open(apath.ToCString(),  O_RDONLY | O_NDELAY | O_CREAT, S_IRUSR);
```

即可
