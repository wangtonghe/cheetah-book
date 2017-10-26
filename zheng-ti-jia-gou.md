# cheetah整体架构

cheetah依赖[httpclient](http://hc.apache.org/)进行请求的封装和下载，使用[jsoup](https://github.com/jhy/jsoup/)进行网页解析，使用[fastjson](https://github.com/alibaba/fastjson)进行json的转换操作。



项目使用[slf4j](https://github.com/qos-ch/slf4j)日志框架，您可以自由选择其他日志框架与之配合。



cheetah分为3个子模块。分别是`cheetah-core`、`cheetah-datastore`、`cheetah-sample`。介绍如下：

* **cheetah-core**

  cheetah核心包，包括下载器、选择器、结果处理器等爬虫基本元素。

* **cheetah-datastore**

  cheetah的数据处理与储存模块。你可以选择合适的储存介质。目前支持`ElasticSearch`、`Redis`储存。

* **cheetah-sample**

  爬虫示例，如[知乎](https://www.zhihu.com/)、[豆瓣电影](https://movie.douban.com/)、[网易云音乐](http://music.163.com/)等知名网站的爬虫demo。



