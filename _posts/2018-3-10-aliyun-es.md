---
title: 阿里云ElasticSearch教程
category: tech
description: 详细介绍阿里云ElasticSearch与Kibana的开通与使用步骤。
# comments: false
keywords: aliyun, ElasticSearch, Kibana
---

Elastic公司的“ELK”是目前最火的日志分析三剑客，其中ElasticSearch负责日志的索引，Logstash负责日志的收集，Kibana负责日志的展示和分析。

事实上并不局限于日志分析，ELK技术栈同样适用于通用的数据采集、索引、分析业务。不可谓不强大。

Elastic与阿里云达成了合作伙伴关系，提供“阿里云 Elasticsearch”的新服务，提供了开箱即用的Elasticsearch和Kibana服务，方便我们快速搭建数据分析平台。下面我们将手把手演示一下如何在阿里云上创建并使用ElasticSearch服务。


首先，我们创建阿里云Elasticsearch服务，阿里云提供了一个月的免费体验服务。

如图，选择自己服务器所在的区域、VPC以及下面的虚拟交换机。对于大规模集群，可以勾选dedicated master，专用主节点可增强群集的稳定性。

![1.png]({{ site.image_url }}{{ page.id }}/1.png)

阿里云目前提供了5.5.3_with_X-Pack版本的ElasticSearch，集成了X-Pack扩展，用户不必再手动购买。

> X-pack是Elasticsearch的一个商业版扩展包，将安全，警告，监视，图形和报告功能捆绑在一个易于安装的软件包中。它可以作为插件被快速集成在Kibana中，并提供给用户集群启用认证、角色权限管控、集群实时监控、可视化报表生成、机器学习等能力。

开通完成后，需要等待3-5分钟进行初始化。初始化完成后，可以点击实例ID，进入管理页面。

![2.png]({{ site.image_url }}{{ page.id }}/2.png)

管理界面显示了ElasticSearch实例的基本信息，并且提供了重启、扩容、续费等操作。
同时可以从这里进入Kibana控制台以及监控页面。

![3.png]({{ site.image_url }}{{ page.id }}/3.png)


## 监控

监控页面已经和阿里云的云监控整合在一起，使用方式和云监控完全相同。

![4.png]({{ site.image_url }}{{ page.id }}/4.png)

譬如在这里我们可以设置一条报警规则，集群的磁盘使用率5分钟的平均值大于80%则报警。还可以配置通知方式，可以选择手机、邮箱、旺、钉钉机器人等。

![5.png]({{ site.image_url }}{{ page.id }}/5.png)


## ElasticSearch

购买ElasticSearch后，在同一区域相同VPC下的服务器便可以直接访问ElasticSearch，相关API可以参考[官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/index.html)

登录到机器上，调用HTTP API来测试一下是否可以连通ElasticSearch

![6.png]({{ site.image_url }}{{ page.id }}/6.png)

查看节点信息：

![7.png]({{ site.image_url }}{{ page.id }}/7.png)

查看index信息：

![8.png]({{ site.image_url }}{{ page.id }}/8.png)


可见，阿里云ElasticSearch已经帮我们已经默认创建了几个系统index，这其实是集群的监控信息。

我们来查看一下其中一个index：

![9.png]({{ site.image_url }}{{ page.id }}/9.png)

可见，这里记录的是Kibana的监控信息，同理`.monitoring-es-6-2018.xx.xx`里面记录的是ElasticSearch的监控信息。


ElasticSearch的使用方式非常简单，我们可以创建一个新的index来体验一下。

创建一个名为aliyun_es的index：

```bash
curl -XPUT -u elastic:YOUR_PASSWORD es-cn-xxxxxxxxxxxxx.elasticsearch.aliyuncs.com:9200/aliyun_es
```

![10.png]({{ site.image_url }}{{ page.id }}/10.png)

index中的每条记录，称为document（下文简称doc），类似于MongoDB中的概念，事实上也很类似，因为每个doc都是一个json对象。
ElasticSearch的灵活之处就在于可以灵活地支持各种结构的数据，同一个index下，doc也可以有不同的structure，但最好不要这么做，这样很影响效率。

在index下，还可以用Type对doc进一步分组，以区别不同类型（无需单独创建，添加数据时指定路径即可）。

添加数据的方式非常简单：

```bash
curl -X POST -u elastic:YOUR_PASSWORD es-cn-xxxxxxxxxxxxx.elasticsearch.aliyuncs.com:9200/index_name/type_name -d '{...}' 
```

我们创建如下几个doc试试。

```json
{
    "title":"hahaha",
    "body":"I am very happy.",
    "author":"admin"
},

{
    "title":"foo",
    "body":"bar",
    "author":"admin"
},

{
    "title":"hello",
    "body":"helloworld",
    "author":"root"
}
```

然后再查询一下，验证数据已经插入成功。

![11.png]({{ site.image_url }}{{ page.id }}/11.png)

*但是Type功能正在被逐渐废弃，预计7.x版本将不再支持Type，因此最好不要再使用。*


虽然只有三条数据，但是道理是相通的，此时我们已经可以开始对数据进行搜索和分析了。



## Kibana

不过使用命令行方式操作ElasticSearch很不方便，并且只有非常基础的功能，我们可以使用Kibana来进行搜索以及数据分析。

点击页面上的按钮进入Kibana控制台，事实上我们只要记录下这个地址便可以由公网访问Kibana控制台，账号为elastic，密码为之前我们创建实例时设定的密码。

登录后，Kibana会让我们输入index名以进行初始化，输入之前我们创建的aliyun_es：

![12.png]({{ site.image_url }}{{ page.id }}/12.png)

创建完成后，可以进一步进行配置。如果不需要的话，那么就已经全部配置完成了！

![13.png]({{ site.image_url }}{{ page.id }}/13.png)

可以在discover一栏看到我们刚才添加的数据，以及数据的大体统计信息：

![14.png]({{ site.image_url }}{{ page.id }}/14.png)

![17.png]({{ site.image_url }}{{ page.id }}/17.png)

也可以进行各种搜索操作：

![15.png]({{ site.image_url }}{{ page.id }}/15.png)

以及各种展示信息，譬如我们可以绘制一个饼状图，统计不同author的的doc数量：

![18.png]({{ site.image_url }}{{ page.id }}/18.png)

以及可以查看更多更详细的监控信息：

![19.png]({{ site.image_url }}{{ page.id }}/19.png)

## 其他

还可以配合Logstash收集日志，组成完整的"ELK日志分析套装"，当然这也并不是必须的，ElasticSearch并不只局限于日志索引，Kibana也不局限于日志分析。
本文变不再赘述，有兴趣的读者可以自己尝试。

## 总结

阿里云ElasticSearch提供了非常完整成熟的ElasticSearch+Kibana解决方案，用户无需手动运维，只要点点鼠标就可以拥有一个独立的ElasticSearch+Kibana集群。

相比于手动搭建，有如下几点好处：

* 搭建、管理方便，只要点点鼠标就可以搭建完成，并可以很容易地实现扩容
* 独立集群，由阿里云集中管理，并支持快照等功能，稳定性高
* 有X-Pack商业授权，可以使用balabala等高级功能，并默认集成多项插件

不过也有一些缺点：

* 价格较高，最低配的集群一个月也要三百多元
* 版本有限，现在官方已经更新到6.x版本，而阿里云上目前只有5.5.3这一个版本可供选择，定制性稍差
* 目前只支持 华北2、华东1、华东2、华南1 这几个区域，其他区域的用户还需要再等等


总体来看，阿里云ElasticSearch服务更适合最求稳定与便捷性的企业用户。
尤其是业务跑在阿里云上的企业，阿里云ElasticSearch服务将有效降低运维监控的成本，并提升数据分析的便捷性。
