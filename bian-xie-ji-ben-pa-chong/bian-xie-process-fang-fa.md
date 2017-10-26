## 编写process方法

编写process\(\)主要根据各个网站的网页元素。

第一个参数page储存了爬取下来的html页面、该页面url等信息。通过`page.getHtml()`方法可得到页面数据。`Html`对象包含了若干jquery格式的选择器用于获取数据。

第二个参数用于储存爬取而来的结果及待爬URL。

```java
    @Override
    public void process(Page page, CheetahResult cheetahResult) {

        String name = page.getHtml().$(".ProfileHeader-title .ProfileHeader-name").getValue();

        //如果name为空，说明爬取的当前页没有我们要的数据，所以将它过滤，直接返回
        if (StringUtils.isEmpty(name)) {
            cheetahResult.setSkip(true);
            return;
        }
        //解析网页
        Map<String, Object> result = new HashMap<>();
        Selectable follow = page.getHtml().$(".Profile-main .Profile-sideColumn .FollowshipCard .FollowshipCard-counts a");
        int followNum = Integer.parseInt(follow.get(0).$(".NumberBoard-value").getValue());
        int followerNum =  Integer.parseInt(follow.get(1).$(".NumberBoard-value").getValue());
        int answerNum = Integer.parseInt(page.getHtml().$(".Profile-mainColumn .ProfileMain-header ul li").get(1).$(".Tabs-meta").getValue());
        int quesNum = Integer.parseInt(page.getHtml().$(".Profile-mainColumn .ProfileMain-header ul li").get(2).$(".Tabs-meta").getValue());

        //将解析结果放在CheetahResult对象中
        result.put("name", name);
        result.put("answerNum",answerNum);
        result.put("quesNum", quesNum);
        result.put("following", followNum);
        result.put("follower", followerNum);
        cheetahResult.putResult(result);

        //将下次需要爬的url储存在CheetahResult对象中，你可以通俗认为是下一页的url
        List<String> links = page.getHtml().$(".Profile-mainColumn .List > div").get(1).$(".List-item .ContentItem-head .Popover").getLinks();
        links.forEach(link -> {
            String newLink = link + "/followers";
            cheetahResult.addWaitRequest(newLink);
        });

    }
```



