---
title: Rails后端提效_指导思想
date: 2017-04-08 09:30
categories: fullstack
tags:
  - Rails
  - refactor
typora-copy-images-to: ipic
---

在提高网站效能的方法里，[优化前端架构是关键](http://an-lee.pro/2017/03/23/2017-03-23-front_end_is_the_key_to_refactor/)，但这并不意味着，后端就不需要提效了。

在 Ruby on Rails 里，后端提效，有两个主要思路，分别是

- 提升 Ruby 代码的效能
- 提升数据库方面的效能

如何知道自己的网站后端效能呢？有一个 Rails 里的测速神器: [rack-mini-profiler](https://github.com/MiniProfiler/rack-mini-profiler)，安装之后，在开发模式下的网页，会出现一个小 widget，显示网站耗费了多少时间。而且会显示以下的内容

- controller 执行的速度
- view 执行的速度
- SQL 查询了几次
- 为什么 SQL 很慢

效果如下图所示

![84437DC7-6A26-4F23-A42B-420253B81BA2](http://okgqgpbx3.bkt.clouddn.com/blog/2017-04-08-84437DC7-6A26-4F23-A42B-420253B81BA2.png)

从这个表里，可以清楚地看出，一个网站的后端效能，其瓶颈在于**数据库的查询速度**。事实上，在实践上，通常后端速度的瓶颈是这样的

- 90% 概率在于数据库
- 10% 概率在于 Ruby 代码

有了这样的一个认识之后，为了提升网站的后端效能，思路就特别明确了：

- 优化**提取数据库的方式**

