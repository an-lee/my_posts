---
title: 前端提效套路4_采用多个CDN域名
date: 2017-03-25 11:00
categories: fullstack
tags:
  - refactor
---

# HTTP 1.1 的限制

根据 HTTP 1.1 通讯协议，一个网站只能同时对一个 domain 开启两个连线。

如果一个网页上有 48 张图片，那么浏览器只能同时下载两张，也就说，这48张图片需要下载 24 次才能下载完。那么即使服务器速度很快，用户的网络也很快，但由于「 最多同时两个连线」 的协议规定，还是得跑 24 次。

# 提效方法

有什么办法提高这个速度呢？方法就是分成 4 个 domain 来下载。

比如说本来我们全部都是在 cdn.jd.com 上下载的，那么我们可以改成这样

- cdn0.jd.com/images/demo.jpg
- cdn1.jd.com/assets/admin.css
- cdn2.jd.com/assets/admin.js
- cdn3.jd.com/assets/fonts/font-awesome.ttf

主机都是同一个主机，但是网址不一样了，这样就绕过了「最多两个连线」 的限制，实现同时下载 8 个档案。

# Rails 实现

还是在 `config/environments/production.rb` 里，找到相应行，改成

```
config.action_controller.asset_host = "http://cdn%d.jd.com"
```

