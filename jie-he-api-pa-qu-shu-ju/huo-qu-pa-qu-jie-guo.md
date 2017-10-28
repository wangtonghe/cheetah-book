## 获取爬取结果

设置完毕，DoubanMovieDemo类看起来像这样

```java
public class DoubanMovieDemo implements PageProcessor {

    private SiteConfig siteConfig = SiteConfig.create();

    @Override
    public void process(Page page, CheetahResult cheetahResult) {
           // 篇幅问题已省略，可查看sample模块查找源码
    }

    @Override
    public SiteConfig setAndGetSiteConfig() {
        // 篇幅问题已省略，可查看sample模块查找源码

    }


    @Override
    public void processJSON(JsonDataResult jsonData, CheetahResult cheetahResult) {
       // 篇幅问题已省略，可查看sample模块查找源码

    }

    @Override
    public Request updateJSONConfig(CheetahResult cheetahResult, SiteConfig siteConfig) {
       // 篇幅问题已省略，可查看sample模块查找源码

    }


    public static void main(String[] args) {
        Cheetah.create(new DoubanMovieDemo())
                .setHandler(new ConsoleHandler())
                .setHandler(new ElasticHandler("localhost", 9300, "wth-elastic", "cheetah_new", "movie"))
                .run();
    }
}
```

运行时配置储存到ES中，在本机运行一段时间后在kibana数据截图如下

![](/assets/屏幕快照 2017-10-28 15.15.28.png)

