## 爬虫定时器

爬虫定时器即为，指定时间让爬虫定点执行。比如我们想让一个爬虫半夜执行，这时就可以使用爬虫定时器。



### １.实现plan方法

plan\(\)方法也是`PageProcessor`接口的默认方法，当你需要使用定时器时，就需要实现它。

plan\(\)方法很好实现，其实它就是我们前面提到的main启动方法。

仍以`DoubanMovieDemo`为例，它的plan\(\)方法如下

```java
   @Override
    public void plan() {
        Cheetah.create(new DoubanMovieDemo())
                .setHandler(new ConsoleHandler())
                .setHandler(new ElasticHandler("localhost", 9300, "wth-elastic", "cheetah_new", "movie"))
                .run();
    }
```

可以看到，它与之前的main\(\)方法完全一样。



### 2. 开启定时器

接下来就要实现定时器了。

cheetah的定时器为CheetahTimer类，其中只有2个方法。一个为`add()`，用于添加定时任务；一个为`plan()`方法，用于启动定时任务。

*  ` add(Class<? extends PageProcessor> crawler, Date date)`

*    `plan()`

add\(\)方法有两个参数，一个是实现了 `PageProcessor`的爬虫类，一个是日期。指定某个爬虫类何时执行。

设置豆瓣电影爬虫在2017-10-31 的零点执行。

```java
public static void main(String[] args) {
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-M-d H:m:s");
        // 指定一个日期
        try {
            Date date = dateFormat.parse("2017-11-31 00:30:00");
            new CheetahTimer().add(ZhihuDemo.class,date).plan();

        } catch (ParseException e) {
            e.printStackTrace();
        }
    }
```

定时器可以同时添加多个爬虫在不同时间执行，只需调用add\(\)即可。



