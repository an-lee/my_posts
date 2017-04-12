---
title: 前端提效套路3_使用CDN
date: 2017-03-25 10:30
categories: fullstack
tags:
  - refactor
---

# 什么是 CDN

CDN 全称是 Content Delivery Network。我们知道，网站主机的物理位置会影响[用户的访问速度](http://an-lee.pro/2017/03/23/2017-03-23-front_end_is_the_key_to_refactor/)，如果面向中国用户，那么最好选用中国的服务器。但是如果面向全球用户，那么服务器放哪似乎都不是最好的解决办法。如果能够把网站同时架设到全世界的服务器就好了。CDN 就是在一定程度上解决这个问题。

CDN 技术就是针对不同地区的用户，自动提供离他们最近的点下载档案。换句话说，如果把图片放在 CDN 上，就相当于把图片放在了全世界很多个不同的服务器上，无论哪个地区的用户访问网站，都会有一个比较近的主机供他们下载网站图片。

CDN 的工作原理大概是这样的：

- 如果 CDN 上有该档案，直接提供最近点给用户下载
- 如果 CDN 上没有该档案，就让用户从原始网站下载，同时也复制一份到 CDN 上，以供下次使用

# 常用的 CDN 服务

- [Amazon Cloudfront](https://aws.amazon.com/cloudfront/)
- [Cloudflare](https://www.cloudflare.com/)

# Rails 上如何实现

购买了 CDN 服务之后，在 `config/environments/production.rb` 里修改

```
config.action_controller.asset_host = "http://cdn.jd.com"
```

这样，全站的 images/css/js 的网址就会变成

- cdn.jd.com/images/demo.jpg
- cdn.jd.com/assets/admin.css
- cdn.jd.com/assets/admin.js