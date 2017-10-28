## 数据处理方式

cheetah目前支持数据储存的方式有两种，redis和elasticsearch。若要支持数据储存需要引用    `cheetah-datastore`

模块。届时只要在开启爬虫时配置相应的IP及所需项即可。



```java
setHandler(new RedisHandler("127.0.0.1", "name_of_set"))  //设置redis储存方式
```

```java
setHandler(new ElasticHandler("127.0.0.1", port, "cluster", "index", "type")) //设置elasticsearch储存方式
```

handler即为cheetah的数据处理模块。使用爬虫时只需配置一行即可将爬取数据储存到指定位置，十分方便。

