## 断点续爬

### 1. 续爬配置

断点续爬是指从上次爬取的断点开始继续爬取。

这个功能是很有必要的。比如你一个爬取已经运行好几个小时了，突然由于某种原因停止了（断电、遇到异常或其他原因），总不能重新爬吧？如果开启断点续爬，就可以无缝连接上次爬取结果继续爬了。

使用断点续爬很简单，首先需要开启redis。断点续爬需要redis的支持，用于储存断点等数据。其次简单进行配置即可。

在setAndGetSiteConfig添加以下两行代码即可

```java
    openBreakRestart(true) //开启断点重爬，从上次中断的位置开始爬

    setBreakRedisConfig("127.0.0.1"); //配置redis的IP地址，本地使用127.0.0.1即可
```



### 2. 原理解析

断点续爬主要原理是，将解析网页获取到的**待爬结果**储存到**redis中待爬结果集**中。爬取时将本次url从redis的待爬结果集中删除，表示已爬。任意时刻redis存储的待爬结果集都是没有爬取但将来会爬取的。所以一旦程序出现意外退出，下次开始时直接查看redis的待爬结果集，若有则加载本结果集，没有才加载startUrl。

redis的待爬结果集储存在redis的Set集合中。命名方式为 "wait\__url_\__key\_" （字符串）+ 爬虫类名的小写形式，比如DoubanMovieDemo爬虫类，其在redis储存的待爬集合名为 `wait_url_key_doubanmoviedemo`_。感兴趣的同学可以看看 。



在开启断点续爬的同时，cheetah还会把爬取过的url储存到redis中，称为保存过的结果集。命名方式为 

"save\_url\_key\_"\(字符串\)   + _爬虫类名的小写形式。比如DoubanMovieDemo爬虫类，其在redis储存的爬取集合名为 `save_url_key_doubanmoviedemo`_。

