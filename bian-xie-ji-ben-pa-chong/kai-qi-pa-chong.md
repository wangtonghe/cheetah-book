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

