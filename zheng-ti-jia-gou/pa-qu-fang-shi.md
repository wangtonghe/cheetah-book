## 爬取方式

爬取方式即爬虫获取数据的方式。简单来说，主要分为**只需要爬取网页就能得到全部数据**和**爬取网页与API结合**两种方式。对于前者我们比较熟悉，也就是大部分爬虫的工作机制——通过解析网页获取数据。而有一些网站做了一些反爬措施，比如通过前端JS动态渲染的方式填充数据，就不能单纯解析网页了。

而无论怎样隐藏数据，向服务器发送的请求不会变。与API相结合的方式是指观察发送给服务器的请求，从而模拟这些请求得到数据。开启API方式也很简单，设置如下两个参数即可。



这里开启豆瓣电影API方式示例

```java
setStartJSONAPI(true) //设置开启JSON API 方式

setJsonAPIUrl("https://movie.douban.com/j/new_search_subjects?sort=T&range=0,10&tags=&start=0") //设置API连接

```

另外，cheetah提供了两个方法用于处理API JSON方式：`updateJSONConfig()`和`processJSON()`前者用于实时更新API URL的值，后者用于处理API JSON方式的获取结果。

```java
 public void processJSON(JsonDataResult jsonData, CheetahResult cheetahResult)
```

```java
 public Request updateJSONConfig(CheetahResult cheetahResult, SiteConfig siteConfig) 
```

具体用法会在稍后的章节介绍。

