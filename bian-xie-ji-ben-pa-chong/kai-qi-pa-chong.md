## 开启爬虫

开启爬虫很简单，写个主函数即可。

```java
   public static void main(String[] args) {
        Cheetah.create(new ZhihuCrawler()) //传入爬虫类示例对象
                .setHandler(new ConsoleHandler())  //设置爬取结果处理方法，这里为显示在控制台
                .run();
    }
```

开启后即可在控制台看到爬取到的结果。

当然，也可以像快速开始那样，设置ElasticSearch为储存方式。前提示你已配置好了ElasticSearch。

```java
   public static void main(String[] args) {
        Cheetah.create(new ZhihuDemo())
                .setHandler(new ConsoleHandler())
                .setHandler(new ElasticHandler("127.0.0.1", 9300, "wth-elastic", "zhihu_new", "user_data"))
                .run();
    }
```



