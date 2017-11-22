---
title: 钉钉已读回执拦截器
category: tech
description: 拦截钉钉消息已读回执
# comments: false
keywords: dingtalk
---

### 更新

***维护公司利益，不再提供下载***

<del>**适用于网页版钉钉**</del>

### UPDATE

经同事提醒，钉钉客户端（3.x）就是Electron包装的一层网页版而已。于是经过简单改造，现在已经支持钉钉客户端。

Mac 用户（3.x版本）可以直接运行`sh install.sh`，自动安装并注入。

Windows 用户可以参考下，把 js 文件拷贝到钉钉目录（大概是叫`../current/web_content`）下，并在`index.html`里加载此 js 。

**注意，一定要放在`app.js`前**

这样OK：

```html
<script src="./assets/ding_interceptor.js"></script>
<script src="./assets/vendor.js"></script>
<script src="./assets/app.js"></script>
```

这样不OK：

```html
<script src="./assets/vendor.js"></script>
<script src="./assets/app.js"></script>
<script src="./assets/ding_interceptor.js"></script>
```

默认会自动拦截全部已读回执。不过此次特增加了“原谅模式”，即恢复默认设置不再拦截，正常向对方展示已读状态。

右击标题栏部分即可激活或关闭。

![header](https://images.lxiange.com/posts/dingtalk-interceptor/header.png)

可以看到，标题栏会变成原谅色，喜欢你们喜欢。

不过值得庆祝的是，钉钉4.0版本已经重写为原生APP，性能和功能都得到很大的提升，可喜可贺！

以下为旧内容：

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

项目地址：<del>[dingtalk-interceptor](https://github.com/lxiange/dingtalk-interceptor)</del>（已删除）


附上README：

### 安装方法：

1. 通过Chrome Webstore安装<del>[DingTalk Interceptor](https://chrome.google.com/webstore/detail/dingtalk-interceptor/dcefpnhobgebmafmamokafniilmmcgdp)</del>（已下架）
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
