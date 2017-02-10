---
title: 增加「下拉选单选项」后出错
categories: fullstack
tags:
  - RoR
---

教程步骤：

> Rails 第三课： Rails 101
> [8-2 可以看到自己参与的所有群组](https://fullstack.xinshengdaxue.com/posts/87)

操作完 Step 3，刷新页面出错

![Screen Shot 2016-12-15 at 6.10.12 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1208598/hgWtbNnSC2gQ3g9vopbR_Screen%20Shot%202016-12-15%20at%206.10.12%20AM.png)

返回到 Step 2，教材中并没明确新增的代码放在哪里，我自以为是地放了一个地方

```ruby "config/routes.rb"
namespace :account do
  resources :groups
end
```

![Screen Shot 2016-12-15 at 6.13.39 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1208598/zLBtpjxSzNrIME7cw8og_Screen%20Shot%202016-12-15%20at%206.13.39%20AM.png)

可能是放错地方了？修改一下变成这样试试看

![Screen Shot 2016-12-15 at 6.14.26 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1208598/M6gqtUzRQ0643akVL8PA_Screen%20Shot%202016-12-15%20at%206.14.26%20AM.png)

在刷新[http://localhost:3000/](http://localhost:3000/)，改对了:)

![Screen Shot 2016-12-15 at 6.15.08 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1208598/ykkeywITUZNPhCP8ZT9w_Screen%20Shot%202016-12-15%20at%206.15.08%20AM.png)
