---
author: hesicong
comments: true
date: 2006-12-01 23:31:01+00:00
layout: post
slug: window-forms-application-multi-language-support
title: Window Forms应用程序多语言支持
wordpress_id: 96
categories:
- 其他
tags:
- c
- 多国语言
---

最近有想法准备研究一下可以实时切换而且方便更改的多国语言的应用程序，在网络上搜索了一些资料，参考了MSDN的一些资料，最终做出一个简单的类用于多语言支持。
**注：该思路和类参考了《C#的Windows编程中多语言的实现》一文，对其作者表示感谢。
另外顺便鄙视一下那些胡乱转载的网站，连作者名字都给胡乱换了！！！ **
![](http://www.hesicong.net/pjblog/WordPics/120106_0821_WindowForms1.png)![](http://www.hesicong.net/pjblog/WordPics/120106_0821_WindowForms2.png)

基本思路是比较简单：

1. 在切换语言的时候调入相应XML资源到hash table
2. 修改界面的时候获取所有界面元素
3. 从hash table里面查找相应的值
4. 赋值即可

难点在于获取所有界面元素。

对于Windows Form应用程序，访问Form.Controls可以得到窗体包含的控件。循环遍历所有的控件并得到其Type，然后对不同的Type进行不同的处理。有些控件可以包含更多的控件，意味着需要用一个递归调用来遍历所有的控件，在我的程序中调用SetSubControls这个子程序来做这个工作。

对于需要显示的Message，我也在XML文件中做了定义。每个Message有一个ID号，根据不同的ID号来区分所要显示的内容。各个语言之间的ID号相同，只是其内容不同而已。

此方法的优点在于

1. XML文件中只需提供所在的窗体和控件的名字即可，方便编辑和调试。
2. 可以方便的创建更多的语言，在不修改程序的情况下可以满足用户自定义语言的需要。

根据以上的思路可以设计一个Localization类，专门负责语言的切换，包含如下的几个函数(具体请参阅源代码)

```
public static void SetLanguage(string lang)    //设置全局语言
public static string GetMessage(string ID)    //得到相应的消息
public static void SetForm(Form form)         //为form设置语言
private static void SetDropDownItems(ToolStripItemCollection items, Hashtable table)    //处理菜单的DropDown Items
private static void SetSubControls(Control.ControlCollection controls, Hashtable table)    //处理子控件
private static Hashtable ReadWindowResource(string frmName, string lang)        //从xml文件里面读取资源
private static void ReadMessageResource()    //读取消息资源
```

以上的几个静态函数就构成了类的基本，调用的时候只需SetLanguage，然后SetForm即可。

示范XML文件：

```
<?xml version="1.0" encoding="utf-8" ?>
<Resources>
<Form>
<Name>frmMultiLanguageDemo</Name>
<Controls>
<Control name="btnEN" text="EN" />
<Control name="btnCHN" text="CHN" />
<Control name="txtCurrrentLang" text="English" />
<Control name="lblText" text="Label"/>
<Control name="chkBox1" text="CheckBox"/>
<Control name="tabPage1" text="Page 1" />
<Control name="tabPage2" text="Page 2" />
<Control name="
;radioButton1" text="Option" />
<Control name="btnShowMsg" text="Show Message" />
<Control name="btnShowWin" text="New Window" />
<Control name="mnuFile" text="File" />
<Control name="mnuExit" text="Exit" />
<Control name="mnuHelp" text="Help" />
<Control name="mnuOptions" text="Options" />
<Control name="mnuOption1" text="Option 1" />
</Controls>
<Name>frmNewWindow</Name>
<Controls>
<Control name="txtText" text="New Window" />
</Controls>
</Form>
<Messages>
<Message id="MSG_TEST" text="Test Message" />
</Messages>
</Resources>
```
当然，这个类还是比较弱，对于自定义的控件循环遍历的方法还有待改进，XML文件的储存结构也需要更进一步的优化，修改Windows Forms的控件时不能同步更新语言资源等等问题还需要解决。

[点击此处下载源代码和演示程序](http://www.hesicong.net/pjblog/wbc_showimg.asp?file=attachments/month_0612/22006121162323.rar)
