## 设置储存方式

在上面的例子中，我们的确得到了知乎关注人数数据，然而只输出到了控制台。无法有效分析及利用数据。这一节我们将以elasticsearch为例，尝试将数据储存起来。

在本机开启一个ElasticSearch的实例（关于ElasticSearch的介绍及安装详见[Elastic中文网站](https://www.elastic.co/cn/) ）。其默认节点间访问端口为9300，则可以在开启爬虫的方法中添加一行

```java
setHandler(new ElasticHandler("127.0.0.1", 9300, "wth-elastic", "zhihu_new", "user_data"))
```

这里设置了ES集群名为`wth-elastic`，索引为`zhihu`_`new`，类型为`user_data`_它会寻找`wth-elastic`集群，并将数据储存到指定索引的类型下。

运行爬虫的方法则如下所示

```java
   public static void main(String[] args) {
        Cheetah.create(new ZhihuDemo())
                .setHandler(new ConsoleHandler())
                .setHandler(new ElasticHandler("127.0.0.1", 9300, "wth-elastic", "zhihu_new", "user_data"))
                .run();
    }
```

开启爬虫后一段时间后，在[kibana](https://www.elastic.co/products/kibana)（Elastic套件之一，用于分析数据）截图数据如下

![](/assets/屏幕快照 2017-10-28 12.59.56.png)

