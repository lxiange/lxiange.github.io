---
title: 钉钉已读回执拦截器
category: tech
description: 拦截钉钉消息已读回执
# comments: false
keywords: dingtalk
---

**适用于网页版钉钉**

### 背景

钉钉的已读回执是一个可以提升办公效率的功能。
可惜相比于“钉一下”采取的主动发送已读回执的策略，普通消息系统直接默认发送已读回执有些时候确实会给人带来困扰，特殊情况下甚至会引起不必要的猜疑与误会。

因此拦截钉钉默认发送已读回执某种程度上可以算是一个刚需了。
实现起来也并不复杂，尤其是对于网页版钉钉而言。
但是令人诧异的是这么一个容易实现的“刚需”，却没有找到现成的工具，因此写个小插件方便一下大家。

插件功能比较简单，就是拦截所有已读回执，本来还想加上各种复杂功能。
譬如指定屏蔽某人消息回执，指定屏蔽包含某些内容的消息回执，甚至指定发送某条消息的回执。
但是仔细想一下，这些功能貌似大多数情况下都用不上，是伪需求。
因此只专注做好一件事就足够了。

项目地址：[dingtalk-interceptor](https://github.com/lxiange/dingtalk-interceptor)


附上README：

### 安装方法：

1. 通过Chrome Webstore安装[DingTalk Interceptor](https://chrome.google.com/webstore/detail/dingtalk-interceptor/dcefpnhobgebmafmamokafniilmmcgdp)
2. 若无法访问Chrome Webstore，可下载此项目解压，然后打开`chrome://extensions/`，点击`加载已解压的扩展程序...`按钮，选择此项目文件夹。

### 使用方法：

安装插件后，登陆网页版钉钉正常使用即可。

代码工作正常情况下，打开控制台，会打印
```
Injecting DingTalk hooks.
Using wsHook. All WebSocket connections are being hooked.
```
这两行信息，表示代码已经注入成功：
![welcome]({{ site.image_url }}{{ page.id }}/welcome.png)
（这堆warning/error是钉钉代码的锅）

此外，每拦截一条已读回执，会在控制台打印一条信息：
![log]({{ site.image_url }}{{ page.id }}/log.png)

此插件会拦截网页版钉钉的所有已读回执，如需发送已读回执，请在手机等其他客户端上再次浏览消息即可。

### 注意事项：

本插件采用简单匹配法，拦截所有发送的包含`updateToRead`的消息。
因此如果你发送的消息中含有此字符串，也将被误伤拦截。
这是我特意留的一个Bug，:P

### 致谢
* 感谢 @启鸣 不停给我发消息做测试。答应了算他一份的，差点忘了。
* [skepticfx/hookish](https://github.com/skepticfx/hookish)
（其实基本上就是照着抄的）
