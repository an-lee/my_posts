---
title: 表单中的user和user_id
date: 2017-02-11 05:45
categories: fullstack
tags: 
  - Rails
typora-copy-images-to: ipic
---

做到「创建订单」这一步时，出现错误

![Screen Shot 2017-02-11 at 5.49.26 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-02-10-215023.png)

根据错误提示，我找到`orders_controller` 里这一行

```ruby
@order.user = current_user
```

确实跟教程上的一样。但我回去看 order 的 `schema.rb` 时，却发现

```ruby
  create_table "orders", force: :cascade do |t|
    t.integer  "total",            default: 0
    t.integer  "user_id"
    t.string   "billing_name"
    t.string   "billing_address"
    t.string   "shipping_name"
    t.string   "shipping_address"
    t.datetime "created_at",                   null: false
    t.datetime "updated_at",                   null: false
  end
```

orders 这个表单里，没有 `user` 这一列，有的是 `user_id`。我就想是不是教程写错了。于是我把 controller 中的 `@order.user = current_user`  改成了 `@order.user_id = current_user` 。

保存后刷新网页，点击「创建订单」，没有出现错误提示了，但也没有创建订单，实际上是什么也没发生。按照代码，创建订单之后，应该跳转到订单的详细页面，也就是 `order_path(@order)` 。

程序没有出现预想结果，但又没有错误提示，这种情况是最难 debug 的了。

我只好回过头去检查每一步。

结果发现在 `order.rb` 里的错误

![Screen Shot 2017-02-11 at 6.11.49 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-02-10-221204.png)

我把 `:user` 错写成了 `:userr`。其实在错误提示里也有体现，错误信息是这么写的

```
Did you mean? userr= user_id= userr
```

这里就出现了 `userr` ，这明显是个错误拼写，我应该第一时间反应过来，哪里拼写错误了。

这个错误改过来之后，一切就正常了。

但是，另一个问题来了，为什么在创建 order 这个 model 时，表单中的列名是 `user_id` ，但在实际调用的时候却是用 `user` 呢？