---
title: 回车与换行的本质
category: tech
description: 由实习期遇到的一个离奇BUG引发的对于回车符与换行符的本质的思考。
keywords: 回车, 换行, 本质
---

看起来这是一道非常简单的送分题。

问到回车和换行符有什么区别，
估计绝大多数人都能说出
“回车符是\r，换行符是\n。Windows下换行用的是\r\n，Mac下是\r，Linux下是\n”
这种没任何用的冷知识（并且Mac下早就不是\r了，教材年久失修啊）。

网上也有很多介绍回车符和换行符的帖子，譬如阮一峰老师的
[这篇博文](http://www.ruanyifeng.com/blog/2006/04/post_213.html)
讲的就很详细，同时也介绍了在不同系统下混用回车换行可能造成的影响。

但是事实上，光背概念或是死记这些“表现”，一点用都没有。
用高中老师的话说：“题目稍微变一变就要抓瞎。”

尤其是这些基础概念，看起来非常简单，很容易让人自负，认为自己的理解一定没错。
不过一旦栽跟头，往往会把三分钟的快餐变成三天的DEBUG盛筵。

很遗憾，在网上没有找到比较好的介绍回车与换行本质概念的文章。
因此特作此文来简单科普一下这个既简单又容易犯错的概念。

## 亲身案例

在阿里实习期间，mentor分配给我一个任务：
将后台吐出的日志（`stdout`的内容），输出到浏览器前端页面中。

乍一看，这是一个非常简单的工作，但是实际操作中却发现了比较诡异的问题。

举个栗子，在安装或下载东西时，terminal中的会动态更新百分比，到百分百后再继续往下执行，就像这样：

```
(Reading database ... 10%
```

随着时间推移，这行内容会变成

```
(Reading database ... 95%
```

最后，过程中的这些百分百信息是不会留在屏幕上的，屏幕上只留下一行总结性的信息。
然后继续向下执行，并输出后续内容：

```
(Reading database ... 69510 files and directories currently installed.)
Preparing to unpack .../libexpat1_2.1.0-7ubuntu0.15.10.2_amd64.deb ...
...
```

然而，如果直接读取stdout内容并输出到前端，和terminal里显示的内容完全不一样，会得到这么一坨东西：

```
(Reading database ... ^M(Reading database ... 5%^M(Reading database ... 10%^M(Reading database ... 15%^M(Reading database ... 20%^M(Reading database ... 25%^M(Reading database ... 30%^M(Reading database ... 35%^M(Reading database ... 40%^M(Reading database ... 45%^M(Reading database ... 50%^M(Reading database ... 55%^M(Reading database ... 60%^M(Reading database ... 65%^M(Reading database ... 70%^M(Reading database ... 75%^M(Reading database ... 80%^M(Reading database ... 85%^M(Reading database ... 90%^M(Reading database ... 95%^M(Reading database ... 100%^M(Reading database ... 69510 files and directories currently installed.)^M
```

写入到文件中，随随便便就超过1mb。

更诡异的是，用vim和用cat读取这个文件，竟然得到了完全不同的结果。
用vim可以看到那一坨过程中的百分比信息，而cat得到的是terminal里一样的显示效果。
无论是在Windows还是在Linux下都是这样。

甚至用od工具以二进制的方式来逐字符检查也未能发现问题根源 =_=

最后挣扎许久，才发现原来是自己对回车与换行的概念没有弄清。

## 回车/换行符的本质以及换行的概念

回车符/换行符起源于老式打字机。当打字机打了一行字需要换行时，需要同时按下CR和LF这两个键（现在的键盘上已经没有这两个键了）。
CR负责将当前的游标从行尾移到行首，LF负责将游标移到下一行。
很容易想象，在老式打字机上，CR+LF组合使用，才可以正确地换至下一行的行首继续打字。

而在电脑上，则完全没有这个必要，只需要告诉电脑需要换行，程序便自动将游标移至下一行的行首。
因此只需要一个“换行标记”的概念，告诉电脑需要换行即可。

但是选择什么来表示“换行标记”这个概念呢？重新定义一个叫“换行标记”的字符？ASCII表已经够挤了，这样很不环保（不过在UNICODE里，确实是有NEL/LS这些特定用途的字符）。于是各个操作系统选择了不同的方式来表示换行：Windows将`\r\n(0x0D0A)`视为换行标记，Linux/Unix将`\n(0x0A)`视为换行标记。我也可以写个操作系统，定义任意某个字符为换行标记，遇到这个字符便换行。

可见，回车符换行符和“换行标记”是完全不同维度的概念。前者是具体的字符，后者是抽象的表示换行的概念。

此外，高级语言中的`\n`，往往指的就是“换行标记”。在不同平台下会自动进行转换，而并不一定对应着换行符`(0x0A)`。


## 回车符的妙用以及输出设备

但是，即使会被当作“换行标记”，CR和LF它们的本身意义却依然还在：

> **“CR负责将当前的游标从行尾移到行首，LF负责将游标移到下一行。”**

所以，如果单单回车，但是不换行，不就覆盖了当前行的内容了么？此外，回车只是将游标移至行首，并不会擦除当前行内容。

例如这行Python代码，后边的world覆盖了前面的hello这几个字符：

![cr_console]({{ site.image_url }}{{ page.id }}/cr_console.png)

通过这一特性，就可以实现很多有意思的特性，譬如动态显示百分比——只要不停用新的百分比覆盖旧的内容即可！

但是注意，要实现这一特性，必须要求输出的终端是“可擦写”的。
而有些终端只能向后添加内容，已有内容不能删改。

例如相同的代码，在IDLE里就是不一样的输出效果：

![cr_idle]({{ site.image_url }}{{ page.id }}/cr_idle.png)

因此，也就不难理解为何相同的内容，直接在终端里显示与用vim打开或者打印到网页上是不同的效果了！


但是也只有回车符保留了老式打字机中的效果。单单换行不回车的话，系统还是会将游标定位到行首，而并不会悬空在行中间。（不然岂不怪怪的？）

## 结语

貌似这篇文章要虎头蛇尾了，因为知道原理后，之前遇到的那个问题就很好解决了：
以\n为换行标记，然后只保留每一行最后一个\r后的内容即可，因为前面的都是原本应该被覆盖掉的内容。

并且，以后不要问“回车和换行符有什么区别”这种空洞的话题了，
问应试者如何在交互式命令行下实现一个可动态更新的进度条就足够了 :)
