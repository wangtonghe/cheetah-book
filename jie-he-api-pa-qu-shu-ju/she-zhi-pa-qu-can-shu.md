## 设置API爬取

我们创建爬取豆瓣电影的爬虫类为`DoubanMovieDemo`，同知乎爬虫一样，`DoubanMovieDemo`需要按要求实现`PageProcessor`。有关解析网页的代码就不贴了，这里主要讲解下有关API JSON的设置方法。

### 开启API方式

开启JSON API 方式需要在爬取参数中设置

```java
    @Override
    public SiteConfig setAndGetSiteConfig() {
        this.siteConfig.setDomain("https://movie.douban.com")
                .setStartUrl("https://movie.douban.com/subject/1292052") //设置起始URL
                .setUserAgent("Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.98 Safari/537.36")
                .addCookie("bid", "PI0P2w4aMDI")
                .addHeader("Accept", "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8")
                .addHeader("Accept-Encoding", "gzip, deflate, sdch, br")
                .addHeader("Accept-Language", "zh-CN, zh; q=0.8, en; q=0.6")
                .setThreadSleep(2000)
                .setThreadNum(3)
                .setJsonAPIUrl("https://movie.douban.com/j/new_search_subjects?sort=T&range=0,10&tags=&start=0") //设置 JSON API的URL
                .setStartJSONAPI(true); //开启 JSON API方式
        return siteConfig;
    }
```

最后两行的设置开启与设置JSON API方式。需要注意的是，由于项目结构限制，即使爬取URL全部来自API方式，也需要设置一个起始URL。如第一行设置那样。

### 处理API方式处理请求与结果

#### 

接下来就是设置`updateJSONConfig()`和`processJSON()`

这两个方法是在`PageProcessor`中声明的，不过它们是java8的默认方法，可以在需要的覆盖。在这里我们便需要重新实现它们。

#### 1.  updateJSONConfig

主要用于更新API JSON的请求url的更新。在本例中就是将

```
https://movie.douban.com/j/new_search_subjects?sort=T&range=0,10&tags=&start=0
```

中的start字段在每次请求后加10。

```java
    @Override
    public Request updateJSONConfig(CheetahResult cheetahResult, SiteConfig siteConfig) {
        String url = siteConfig.getJsonAPIUrl();  //获取API url

        //对url进行修改
        String numStr = url.substring(url.lastIndexOf("start="));
        int num = Integer.parseInt(numStr.substring(6));
        String newUrl = url.replace(numStr, "start=" + (num + 10));

        siteConfig.setJsonAPIUrl(newUrl);  //将修改后的url重设

        return new Request(newUrl, null, RequestMethod.GET);  //返回下次API请求时的请求封装
    }
```

 如注释所言，updateJSONConfig\(\)方法其实做了两件事，**重设API URL和构建下次API请求**。**在cheetah内部，每次的网页爬取解析后便是API JSON的爬取。**updateJSONConfig第一个参数即是当前次网页解析的结果（也就是process\(\)方法储存到CheetahResult

 类的结果）。updateJSONConfig\(\)可以选择是否使用这些结果构建下次API请求的URL，本例中显然不需要，因为我们只需要将上次URL的start字段加10即可，不需要依赖其他结果。

#### ２. processJSON

此方法只有一个作用即是解析API获取到的数据并处理它们。

在本示例中，我们只需获取结果集中的url并加入待爬列表。

```java
  @Override
    public void processJSON(JsonDataResult jsonData, CheetahResult cheetahResult) {
        List<Map<String, Object>> listData = jsonData.parseListFromMap();

        listData.forEach(eachMap -> {
            String url = (String) eachMap.get("url");
            cheetahResult.addWaitRequest(url);

        });
    }
```



