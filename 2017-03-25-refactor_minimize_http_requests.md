---
title: 前端提效套路2_最小化HTTP请求
date: 2017-03-25 10:00
categories: fullstack
tags:
  - refactor
  - HTTP
---

# HTTP请求周期

浏览器访问网站时，每下载一个档案都要经历一个 HTTP 请求周期：

1. 用户向服务器发出请求 request
2. 服务器判断是否要回传档案
3. 用户下载档案


每下载一个档案就要经历一个周期，每一个周期都要历时 50~200ms。

![WechatIMG3](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-25-013611.jpg)

# 提效方法：压制

因此，提效的一个简单思路就是

> 减少需要下载的档案。

我们在写 CSS 或者 JavaSript 的时候，一般是分开很多个文件写的，这样便于读写，也有利于后期维护，但上线之后却不利于网站的效能。

![Screen Shot 2017-03-25 at 9.35.52 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-25-013613.jpg)

![Screen Shot 2017-03-25 at 9.34.55 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-25-013612.jpg)

所以，在前端届，上线网站时，都会把所有 CSS 文件打包压制成一个文件，把所有 JavaScript 文件打包压制成一个文件。因为

- 多个 CSS 样式放在一个文件里，是可以正常运行的
- CSS 文件里所有空行都「 砍掉」 ，还是可以正常运行的

所以，一般上线的网站，CSS 文件都只有一个，而且只有一行……

在 Rails 中，压制的命令是

```
rake assets:precompile
```

 但是，在 Rails 框架中，部署时会自动压制，一般不需我们自己处理。