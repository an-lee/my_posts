---
title: has_many和belongs_to
categories: fullstack
tags:
  - RoR
---

在 [5-4](https://fullstack.xinshengdaxue.com/posts/73) 里建立了「群组」和「使用者」之间的联系。整个过程好像是这样的

1. 在 group 的数据库里增加一个属性，名为 user_id
2. 在 `user.rb` 里增加 `has_many :groups`
3. 在 `group.rb` 里增加 `belongs_to :users`

逻辑关系应该是这样的，`has_many`表示「使用者」可以有多个「群组」，`belongs_to`表示每个「群组」只属于一个「使用者」。

如果反过来，在 user 里增加 group_id 属性，两者的联系就搞反了，变成每个「使用者」只能属于一个「群组」，但「群组」可以有多个「使用者」，也就是一个「群组」的创建者可以是多个，这显然就不对了。
