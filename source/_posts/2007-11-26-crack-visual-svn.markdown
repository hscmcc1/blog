---
author: hesicong
comments: true
date: 2007-11-26 22:18:43+00:00
layout: post
slug: crack-visual-svn
title: 破解Visual SVN
wordpress_id: 279
categories:
- 其他
---

[](/images/others/a2007112615188.gif)

SVN 1.3.1之前的版本和VS2008有冲突，表现为VS2008的IDE彻底无法使用，即便点击最简单的About都会出现非法操作。而Visual SVN 1.3.1网上还没有破解。按照1.2.2的破解思路，把VisualSVN.Core.dll替换即可。用ildasm查看，VisualSVN.Core.dll是.NET的一个类，用reflector打开以后发现这个类既没有做模糊，也把Licence的东西都写在里面了。

[](/images/others/a2007112615188.gif)![](/images/others/image/thumb/a2007112615188.gif)

SVN 1.3.1之前的版本和VS2008有冲突，表现为VS2008的IDE彻底无法使用，即便点击最简单的About都会出现非法操作。而Visual SVN 1.3.1网上还没有破解。按照1.2.2的破解思路，把VisualSVN.Core.dll替换即可。用ildasm查看，VisualSVN.Core.dll是.NET的一个类，用reflector打开以后发现这个类既没有做模糊，也把Licence的东西都写在里面了。

[](/images/others/a2007112615188.gif)

我就下手了，将VisualSVN.Core反编译，修改了Licencing里面相关的判断函数全部更改了，在进入VS2005里面看看，Visual SVN除了状态是"No Licence"以外，所有的功能正常：)破解完成。
破解文件

[download id="10"]

仅供学习研究使用，谢谢！
