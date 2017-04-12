---
title: API实作的一次debug
date: 2017-03-31 17:30
categories: fullstack
tags:
  - Rails
  - api
typora-copy-images-to: ipic
visible: hide
---

在实作订火车票的 API 时，用 Postman 测试订票过程，总是失败，报错画面如下：

![Screen Shot 2017-03-31 at 6.49.35 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-31-095207.png)

从报错来看，找不到 `Train` ？

第一反映，进 `rails console` 看看

![Screen Shot 2017-03-31 at 5.55.11 PM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-31-095522.png)

明明就有啊。

这个 `create` 的 action 是在 `reservations_controller` 下的，然后我去检查 `reservation` 和 `train` 这两个 model 之间的关系。

```ruby
class Reservation < ApplicationRecord
  # ...略
  belongs_to :train
  # ...略
end
```

```ruby
class Train < ApplicationRecord
  # ...略
  has_many :reservations
  # ...略
end
```

关系没错呀。

再检查 `schema.rb` ，

```ruby
ctiveRecord::Schema.define(version: 20170330213303) do

  create_table "reservations", force: :cascade do |t|
    t.string   "booking_code"
    t.integer  "train_id"
    t.string   "seat_number"
    t.integer  "user_id"
    t.string   "customer_name"
    t.string   "customer_phone"
    t.datetime "created_at",     null: false
    t.datetime "updated_at",     null: false
    t.index ["booking_code"], name: "index_reservations_on_booking_code"
    t.index ["seat_number"], name: "index_reservations_on_seat_number"
    t.index ["train_id"], name: "index_reservations_on_train_id"
    t.index ["user_id"], name: "index_reservations_on_user_id"
  end

  create_table "trains", force: :cascade do |t|
    t.string   "number"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
    t.index ["number"], name: "index_trains_on_number"
  end

end
```

还是没发现那里错了。

于是，我**重做了一遍**。竟然！还是同样的错误！！

我又发现，用 Postman 来查看火车号码的列次的时候是正常的，但用的是 GET 动作。而「订票」这个过程是一个 POST 动作，在 Postman 里是需要输入 params 的。仔细检查过 params 也没错。我开始怀疑 Postman 出问题了。

于是，我抱着试一试的心态，把 Postman 删除之后，重新安装。

然后，**问题解决**了！

反思这个 debug 过程，从报错的提示入手，回溯到程序执行的每一步，检查是否有错误。如果仍然找不到错误，可能不是代码本身的问题，而是外部环境的问题，这个时候就要从软件开始排查。