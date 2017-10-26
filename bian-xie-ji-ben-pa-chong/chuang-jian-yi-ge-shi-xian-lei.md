## 创建一个实现类

所有的爬虫类都需要实现`PageProcessor`这个接口。我们把爬虫类命名为`ZhihuCrawler`,目前像是这样：

```java
public class ZhihuCrawler implements PageProcessor{

    @Override
    public void process(Page page, CheetahResult result) {

    }

    @Override
    public SiteConfig setAndGetSiteConfig() {
        return null;
    }
}
```

这里有两个必须实现的方法：`process()`和`setAndGetSiteConfig()`。

`process()`方法用于解析你需要爬取的网站并处理其结果，`setAndGetSiteConfig`用于设置网站的userAgent、cookie及爬取速度等配置。

