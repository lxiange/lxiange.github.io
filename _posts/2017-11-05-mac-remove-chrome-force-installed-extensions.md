---
title: Mac 下卸载 Chrome 按照企业政策安装的插件
category: tech
description: 在 macOS 系统中，删除 Chrome 按照企业政策安装的插件，并介绍其原理
keywords: Mac, Chrome, macOS, 插件
---

公司配发的电脑，往往会按照公司政策安装一些监控软件。这些监控软件同时也会在Chrome浏览器中安装监控插件，并且这些插件是无法卸载的，就像这样：*此处有图*

我是不反对安装监控软件的，毕竟是公司电脑，受到公司监控无可厚非。不过比较麻烦的是强制安装的这些Chrome监控插件有时会让整个浏览器卡死。经常会遇到浏览器左下方提示“正在等待扩展程序：xxxxx”，并且打不开任何页面的情况。这时必须重启浏览器才能生效。

这类插件严重影响生产力，便想办法干掉。网上搜了一圈，只能找到Windows下的解决方案。macOS下竟然没有找到现成的办法。不过既然我们拥有电脑的root权限，那肯定是肯定做到任何事情的。捣鼓了一圈，终于找到解决方案，分享给大家，希望有所帮助。应该同样适用于卸载其他强制安装的流氓Chrome插件。

## Windows下方法

很容易找到的方法有：

### 修改组策略

[3 Ways to Remove “Installed by Enterprise Policy” Chrome Extension](https://malwaretips.com/blogs/installed-enterprise-policy-removal/)

只看方法一就行了，另外两个方法都要安装额外软件，不推荐

### 修改注册表

[chrome谷歌浏览器扩展无法关闭、删除：此扩展程序受政策控制,无法删除或停用](https://lzw.me/a/chrome-extensions-remove-disabled.html)

Windows下我没有亲身做验证，大家可以适当参考。

## Mac下的方法

### 简单的方法

删除`~/Library/Application\ Support/Google/Chrome/Default/Extensions/`目录下，以该插件ID命名的文件夹。

缺点：有可能自动更新重生。并且插件列表里依然会存在这个插件（虽然实质上已经不存在了），强迫症患者无法忍受。

### 更简单但不一定有效的方法

打开`系统偏好设置->描述文件`，找到包含com.google.chrome的一项，删除之。*此处有图*

缺点：有可能顺带删除了该描述文件中其他配置项，不过可以手动再添加回来。
并且有些监控软件会每次重启时自动再添加描述文件，如果是这种情况，就要逐个击破。

### 针对WebsenseEndpoint插件

以`Websense Endpoint`插件为例。进入`/Library/Application\ Support/Websense\ Endpoint/DLP/`目录，打开`WebsenseEndpointExtension.config`文件，你会发现这里配置了往chrome里配置`ExtensionInstallForcelist`这个键，要求chrome强制安装这个插件。

按道理应该直接删掉这个值即可，但是你会发现即使以root身份也无法编辑。和企业版Windows系统类似，你登录的这个root，是“伪root”，还有一个更高管理员账号。

如下文。一般的root，只是wheel用户组下的root，还有一个更高权限的admin用户组。
（是不是有种公司手上的电脑不由你做主的感觉？）

```text
drwxr-xr-x  3 root        admin    96B  6 27  2017 Websense Endpoint Helper.app
drwxr-xr-x  3 root        wheel    96B  1 27  2017 WebsenseEndpointDLP.kext
-rw-r--r--  1 root        admin   2.3K 11  5 21:40 WebsenseEndpointExtension.config
-rw-r--r--  1 root        admin    11K  1 27  2017 WebsenseEndpointExtension.crx
```

其实破解方式也比较简单：Mac进入恢复模式，然后进入终端编辑即可。

不过需要注意的是，进入恢复模式后，相当于进了一个新的系统。文件系统和原系统不是同一个，因此此时的`/Library`目录不是彼`/Library`，请切换到`/Volumes/Macintosh\ HD/`目录，这个才是macOS下的根目录。

## 原理

无论是Windows还是macOS下，其实强制安装不可卸载的插件都不是chrome的锅，而是取决于系统的设置。chrome只是遵循了系统的设定而已，可以在[chrome://policy/](chrome://policy/)页面看到按照本机配置的策略安装的插件。

其实如果去看Chromium的文档的话，会发现讲得很清楚。Chrome强制安装的插件，是由ExtensionInstallForcelist这个配置参数来决定的。如何修改策略以及更多详细说明，大家可以去看文档，不多赘述：

* [ExtensionInstallForcelist](http://www.chromium.org/administrators/policy-list-3#ExtensionInstallForcelist)
* [Documentation for Administrators](http://dev.chromium.org/administrators/mac-quick-start)
* [Policy Templates](http://dev.chromium.org/administrators/policy-templates)
