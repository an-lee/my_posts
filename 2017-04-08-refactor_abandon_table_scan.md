---
title: 后端提效套路2_TableScan
date: 2017-04-08 11:00
categories: fullstack
tags:
  - Rails
  - refactor
typora-copy-images-to: ipic
---

 # 什么是 Table Scan

从字面来理解，就是「扫描表格」。也就是说，当我要提取一个表格中的符合某个条件的一笔数据时，数据库的执行方法是，按照顺序，一笔一笔数据地去判断，直到找到符合条件的那一笔为止。

用一个形象的例子来比喻，假如我们去图书馆，要找一本《老人与海》。如果用 Table Scan 的方法来找，就等同于，图书馆管理员把整个图书馆的书都倒在地上，一本一本得给你找。

# 索引才是正确办法

显而易见，这种办法是极其低效的。

那应该怎么找呢？当然是依靠索引，也就是 `index` 。图书馆里建立了对每一本书的索引，需要找哪一本书，只需要按照索引来找就好了。

同理，在我们需要调用数据库的时候，也应该用索引的办法，尤其是遇到数据量特别大的情况。

# 如何打索引

比如说 User `has_many` Order 这种情况。

对于 User 来说

```ruby
class User < ActiveRecord::Base
  has_many :orders
end
```

对于 Order 来说

```ruby
class Order < ActiveRecord::Base
  belongs_to :user
end
```

在 Order 这个 model 里，还会有一个 `:user_id` 的栏位

```ruby
def change
  add_column :orders, :user_id, :integer
end
```

在这基础上，为了增加索引，我们还需要再新增一条 migration 来打上 Index

```ruby
def change
   add_index :orders, :user_id
end
```

# 常见的需要 index 的情况

## state_machine 的条件

```ruby
def index
  @new_orders = Order.where(:aasm_state => "order_placed")
end
```

## 时间

```ruby
def index
  @orders = Order.order("created_at DESC")
end
```

## True / False

```ruby
def index
   @paid_orders = Order.where(:is_paid => true )
end
```

