---
title: 个人博客的类型选择
category: tech
description: 有关个人博客的搭建
keywords: 博客, 搭建, 平台, 文章, 鲁棒性
---

有一个精彩的个人博客无疑是一件非常酷的事情。写博客的好处，这里无需多言。

这篇文章也不会去手把手教你如何一步步把博客搭建起来。事实上，你可以很容易地在网上搜到许多不错的教程，所以也就没必要重复造轮子了。

很多时候，选择比努力更重要。这篇文章只想探讨应该选择什么形式来搭建个人博客。如果你还没有搭建个人博客，或者对当前自己的博客不太满意，那么不妨参考一下本文。

目前市面上的博客平台有很多，可以当作博客来用的平台有更多。
有这么多选择，其实是一件非常苦恼的事情。
尤其是博客这种东西，意义比较重大，迁移成本很高，因此需要慎重选择。
事实上关于选择什么样形式的博客我也纠结了很久，也走过许多弯路。
最后算是找到了属于自己的“最佳实践”，因此想把个人的思考与经验分享给大家。

同时这也是本博客的第一篇博文，后边还会用两篇文章分别介绍域名注册与备案，以及静态网站主机与CDN的选择等配套内容。

## 博客平台的评价要素

个人认为，选择博客平台，需要考虑的因素有：

1. 易用程度（写文章/插图片/排版便捷、评论互动、流量统计、版本控制）
2. 可定制性（配置个人喜欢的样式及功能）
3. 个人品牌价值（写博客是为自己创造影响力，而不是在为别人写文章）
4. 搭建/维护/迁移成本（不要太贵，也不要太折腾。此外，如果要迁移博客，尽可能将损失降至最低）
5. 鲁棒性（随时可以访问，并且访问速度快）
6. <del>逼格（个人域名以及https的小绿锁是一定要有滴）</del>


下面就从这几个方面，来粗略分析一下目前常见的几种博客选择。

## 常见的博客类型

### 使用现有平台

现在很多平台都鼓励用户写文章，虽然这些平台的初衷并不是为你托管博客。
但我们仍可以把它们当成博客来写。

它们的有很多共性之处，譬如优点有：

* 写作非常省心，只需要专注于内容就可以，其他东西都不用考虑。

* 搭建维护成本低，很容易开通，并且都是免费的。

* 鲁棒性很强，毕竟这些成熟企业还是**相对**比较可靠的。

* 有平台推广优势，比较容易收获第一批种子读者，这对新人是很有激励作用的。

缺点也都类似：

* 定制性较差，样式和功能基本上完全由平台提供，个人很难做更多改造。因为严格来说，这个博客并不完全是你的，你需要无条件遵守平台的服务条款。

* 迁移成本高，毕竟这些平台都不愿意用户离开。如果要迁移或者备份，基本上只能手动复制文字、下载图片。而文章的排版、读者的评论等内容则是无法保存。

* 个人品牌价值提升有限，不能自定义域名，可能更多地只是收获一些该平台下的关注者而已。比较难打造自己的品牌，粉丝们顶多记住你的ID，并且平台依赖性很强，一旦换个圈子，这些影响力可能就没了。

除了共性特点外，下面再分析几个常见平台的特性。

#### 个人微信公众号

总的来说，微信公众号最大的问题就是太封闭，甚至无法被搜索引擎收录。文章的分享也有很多限制，完全依赖于微信这个生态圈，和互联网开放分享的精神相悖（虽然我是比较佩服张小龙的野心的）。因此，**你的文字很难让更多人受益，你也无法通过博客与更多人交流。**

此外，在微信公众号上写作本身也有很多限制，譬如无法插入外链、电脑端阅读体验差（譬如图片都是动态加载的，电脑端翻页速度快，图片加载跟不上）、没有持久化链接等等。

同时，它对个人品牌价值的贡献也很低。虽然不排除你的公众号运营地非常成功，读者甚多，但即便如此，按照个人博客的定义，它依然是不合格的。

想进一步了解的同学，推荐阅读耗叔的这篇文章：
[《为什么我不在微信公众号上写文章》](http://coolshell.cn/articles/17391.html)

所以，微信公众号基本上就是自娱自乐或者强行拉上亲朋好友娱乐娱乐而已，完全不应该作为个人博客使用。

*净化微信朋友圈，人人有责。别净整些技术文章破坏气氛，多分享些求点赞求投票的链接难道不好？（手动滑稽*

#### 知乎专栏，CSDN，简书等平台

这几个平台相比微信要好一些，它们更加开放，你的文章可以被搜索引擎收录，能够很方便地在整个互联网上分享。

并且依托平台优势，比较容易获得流量，也更容易和读者进行互动。还能蹭平台的热度，在搜索引擎中占据靠前的排名。

目前最常见的就是这类“伪”博客平台，它们的特点基本上可以用前文所述的那些共性的优点和缺点来进行概括。

#### 其他变体

其实这里的“平台”，是一个相对广义的概念，因为这种“平台”有很多表现形式。举个例子：有些同学在GitHub Issue里写博客，某种程度上也属于这一类。
虽然写起来比较方便，但是个人觉得这种形式不太好。

主要缺点在于功能有限、不便管理、无法当作个人品牌去长期维护。此外，备份迁移也都相对比较困难。

这也是很多现有的非专业博客平台的共性缺点。如果你把你的博客看成非常珍贵的私人财富的话，就不要把博客放在这些平台上。

三年河东三年河西。我们见证了太多平台的起起落落：CSDN、博客园日渐式微，知乎、简书方兴未艾。没人能预料到互联网的发展，因此完全没有必要把个人博客的命运和平台绑定在一起。

唯一值得推荐的博客平台，或许就是Google的Blogger了。非常专业，符合了博客的标准定义，还可以绑定自己的域名，对非程序员朋友来说是一个极佳的选择。可惜被封，只得作罢。
*不过按照谷歌的脾气，说不定自己就把Blogger关了*


### 自己搭建博客

自己搭建博客，意味着要自己包办一切，无论是搭建还是维护或是文章的推广，都需要自己来完成。
当然，天下没有白吃的苦头，付出的成本是会有回报的。
并且你要有自信，因为绝大多数“大牛”们，都是采取自己搭建博客的方式。

此外，也不必要太担心麻烦，因为目前有很多软/硬件服务可供使用，并且它们大多还是免费的。
稍微花点时间学习一下，你会发现其实也没有那么复杂，况且学习的过程本身就是值得的。

自己搭建博客的优点有：

* 定制性非常强，只要有足够的动手能力，你可以设定任何样式，配置任意功能。

* 真正属于自己的品牌，值得你一辈子去维护。不仅是为了给别人看，更是自己的宝贵财富。那些知名的大牛们，无不都有自己的独立博客，并坚持更新。

* <del>高逼格</del>

同时嘛，缺点就更多了：

* 使用麻烦。可能要手动同步文章到服务器，还要找单独的服务器存放图片等多媒体资源。

* 推广难，很可能辛辛苦苦花几天写的文章，结果只有爬虫来看。并且用户粘性差，不像平台有“关注”功能。所以读者即使觉得你写得好，也很难去关注你博客的更新。

* 配置麻烦，并且要长期运维，可能要关心很多东西：域名、证书、备案、CDN。。。

* 价格贵，如果要更好的服务，肯定是要花钱的。<del>Pony: 想一想，不充钱你会变得更强吗？</del>

* 鲁棒性差。毕竟是独自维护的站点，可靠性肯定相对差一些。


其中，又可大致分为两种类型：

#### 租借虚拟主机搭建博客

*这里同时包括虚拟主机和虚拟服务器，前者只是托管网站，而后者拥有完整的操作系统。*

前端后端都能定制，只要动手能力足够强，真正的想干啥就能干啥。
如果真想折腾的话，从前端页面到后台数据库全都自己做，说不定还要动手写代码定制一下。

但是易用性较差些（或者说，你要费更大的精力，才能折腾到用起来顺手）。
并且搭建/维护/迁移成本很高，不仅是要花更多钱，时间成本也很大。很可能你最后都忘了本来只是要搭建一个博客而已。

同时，如果是个人的服务器，鲁棒性也较差。为了更好的访问体验，还会去折腾防DDoS、云存储、CDN等等。嗯，这又是一笔精力与金钱。

有人说，这有点杀鸡用牛刀了（我就是想吃个鸡蛋而已，有必要搞个养殖场么？）。

不过我倒是觉得，对于动手能力强的同学，非常值得试一试，毕竟折腾的过程本身就是快乐的 :P


#### 生成静态网站托管

其实，这种方案是以上几种方案之间的Trade Off。

相对于租借主机，舍弃了一些定制性，可供选择的博客系统有限。
并且因为都是纯静态页面，所以没有数据库。因而诸如评论系统、站内搜索等功能，都要另辟蹊径来实现。

不过倒是降低了搭建维护成本，因为市面上目前有很多成熟免费的产品供选择。

把静态博客托管在大公司的平台上，不仅享受到了平台提供的便捷服务、强大功能以及鲁棒性，同时依然可以自定义域名、定制前端页面。看起来确实是一个鱼和熊掌可以兼得的方案。

GitHub Pages号称可以在5分钟之内搭建个人博客，确实非常方便。
并且，如果想让博客的访问体验进一步提升的话，还可以额外去配置CDN、云存储、HTTPS证书等等。同时保留了易用性和扩展性，值得尝试。

<br />

其实，对于自己搭建博客而言，也只有一次配置比较折腾而已，之后的维护成本并不高。

建议你可以通过Markdown来写作，将文章的内容和样式都写在一起，然后由Jekyll等工具自动生成博客页面。

一方面，你可以省去额外的排版、发布工作。
另一方便，在备份/迁移博客时，文章的内容和样式都同时得到了保留。
并且都是纯文本的内容，便于存储与版本控制，可以当作代码库来管理。
这也就意味着你宝贵的博客内容，几乎不可能丢失。

## 结语

本文介绍了选择博客平台时要考虑的几个因素。
也介绍了常见的几种博客形式——使用现有的一些平台或是自己搭建博客。
并且根据列出的那几个评判因素，详细分析评价了选择各种博客形式的优劣之处。

如有打算创建博客或迁移博客，不妨参考一下本文观点。

希望能够对大家有所帮助，也欢迎与大家交流讨论。
