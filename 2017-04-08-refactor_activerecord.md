---
title: Rails后端提效_提升数据库的查询效率
date: 2017-04-08 10:00
categories: fullstack
tags:
  - Rails
  - refactor
typora-copy-images-to: ipic
---

Rails 后端提效的重点在于[提升数据库的查询效率](http://an-lee.pro/2017/04/08/2017-04-08_how_to_refactor_in_back_end/)，而最影响数据库查询速度的两个要素是

- 查询的次数
- 数据库索引的效率

在 Rails 里面，我们是用一套非常强大的工具来调用数据库的，即 ActiveRecord。换句话说，我们并不需要像其他开发者那样直接跟数据库打交道，而是通过 ActiveRecord 这个中介作为我们的代理人去跟数据库打交道。

使用 ActiveRecord 最明显的好处是，避免了繁杂冗长的 SQL 语法。如果我们要直接调用数据库，比如说调取 Post 这个表格的第 12 笔数据，我们要这么写

```sql
SELECT "posts".* FROM "posts" WHERE "posts"."id" = $1 LIMIT 1 [["id,12"]]
```

然而，有了 ActiveRecord 之后，在 Rails 里，我们只需要优雅地写

```ruby
Post.find(12)
```

就可以了。剩下的工作，ActiveRecord 会帮我们完成。大致的关系如下图所示

![WechatIMG1](http://okgqgpbx3.bkt.clouddn.com/blog/2017-04-08-WechatIMG1.jpeg)

得益于 ActiveRecord 的强大功能，在 Rails 里，我们就可以享受到快速开发的红利。

但是，事情往往有两面性，便利的背后也会随之带来一些问题。

这些问题可以简单地理解为，

> ActiveRecord 无法百分百地领悟到我们的真实意图，不会随机应变，只会笨笨地按照已有的老套路执行，从而导致了效率低下。

所以说，后端提效关键在于数据库，数据库提效在于优化使用 ActiveRecord 的查询方法。