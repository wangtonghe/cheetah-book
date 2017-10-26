## 设置爬虫参数



`setAndGetSiteConfig`方法是爬虫的配置方法。含义参看注释。

```java
    @Override
    public SiteConfig setAndGetSiteConfig() {

        SiteConfig siteConfig = SiteConfig.create();

        siteConfig.setDomain("https://www.zhihu.com") //设置网站域名
                .setStartUrl("https://www.zhihu.com/people/zhang-jia-wei/followers") //设置爬虫起始url
                .setUserAgent("Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.98 Safari/537.36")
                .addHeader("Accept", "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8")
                .addHeader("Accept-Encoding", "gzip, deflate, sdch, br")
                .addHeader("Accept-Language", "zh-CN, zh; q=0.8, en; q=0.6")
                .setThreadSleep(2000)  //设置线程休眠时间，单位ms
                .setThreadNum(3);  //设置线程数
        return siteConfig;
    }
```



