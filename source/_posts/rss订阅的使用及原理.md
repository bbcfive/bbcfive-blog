---
title: rss订阅的使用及原理
date: 2020-01-16 23:54:06
tags:  rss
categories: 工具
---
## 一、在Chrome使用rss订阅
1. 打开Google网上商店安装RSS Feed Reader插件
![avatar](https://img2018.cnblogs.com/i-beta/1549437/202001/1549437-20200116233733689-2132255122.png)

2. 浏览器链接栏右侧会有一个rss黄色图标，点击图标即可添加
![avatar](https://img2018.cnblogs.com/i-beta/1549437/202001/1549437-20200116233906911-1807687237.png)

3. 输入想要订阅的网站url并将其添加至rss订阅器里
![avatar](https://img2018.cnblogs.com/i-beta/1549437/202001/1549437-20200116234126123-728412139.png)

4. 可以选择订阅流的更新间隔、通知方式以及一些过滤条件，至此完成订阅
![avatar](https://img2018.cnblogs.com/i-beta/1549437/202001/1549437-20200116234514379-210017291.png)

## rss原理
1. 使用RSS 的 <channel> 元素来描述 RSS feed。
``` xml
<?xml version="1.0" encoding="ISO-8859-1" ?>
<rss version="2.0">

<channel>
  <title>W3School Home Page</title>
  <link>http://www.w3school.com.cn</link>
  <description>Free web building tutorials</description>
  <item>
    <title>RSS Tutorial</title>
    <link>http://www.w3school.com.cn/rss</link>
    <description>New RSS tutorial on W3School</description>
  </item>
</channel>

</rss>
```

`channel` 元素来描述 RSS feed，需要有三个必须的子元素：
* `title` - 定义频道的标题。（比如 w3school 首页）
* `link` - 定义到达频道的超链接。（比如 www.w3school.com.cn）
* `description` - 描述此频道（比如免费的网站建设教程）
* `channel` 通常包含一个或多个 `item` 元素。每个 `item` 元素可定义 RSS feed 中的一篇文章或 "story"。

2. 阅读器去读取web feed
网站联合化是透过（web feed），让所谓的阅读器（reader）读取资料。一些人以为web feed像是电视和收音机一样把资料送过来；但事实上，web feed只是一个类似网站的的档案，从头到尾都放在源网站上，但会定期把网站的更新内容写进来。等使用者打开阅读器时，阅读器便会读取feed的内容，再判断有哪些新的东西存在。
许多关于联合化技术的介绍会说这是一种推送技术（push technology）；也就是网站主动把资料传给订阅者，而不是由使用者去搜寻资料。不过网站实际上虽然发布的文章，却并不把资料丢给它们，而是让阅读器去读取web feed。订阅者所要做的就是记住feed网站就行了。

#### 参考文献：
1. [https://www.cnblogs.com/ShaneZKY/articles/3525116.html](https://www.cnblogs.com/ShaneZKY/articles/3525116.html)
2. [https://www.cnblogs.com/zuge/p/6297221.html](https://www.cnblogs.com/zuge/p/6297221.html)