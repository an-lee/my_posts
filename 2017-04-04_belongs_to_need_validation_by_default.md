---
title: belongs_to默认需要validation
date: 2017-04-04 14:30
categories: fullstack
tags:
  - Rails
---

在实作中发现一个问题，当用了 belongs_to 来连结两个 model 之后，用于连结的那个栏位不能为空值。比如说 Rails 101 里面的 group 和 post。为 Post 增加一个栏位 :group_id 作为连结，然后在 model 里定义连结关系。

```ruby
class Post < ApplicationRecord
  belongs_to :group
end
# ---
class Group < ApplicationRecord
  has_many :posts
end
```

在这种情况下，创建 Post 时，如果 `:group_id` 为空值，新 post 是不会被保存的。也就是说，New post 被保存之前会去验证 `:group_id` 这个栏位。

如果在 controller 里 create post 时用了 `save!` 方法，就会有报错

```
ActiveRecord::RecordInvalid: Validation failed: Group must exist
```

如果用了 `save` ，情况就会是什么事情都没发生，但是 create post 失败了…

现在考虑另外一种情况，比如说我原来公司的架构，大概是

> 总部 >> 运营中心 >> 事业部 >> 专业部门

作为一名员工，可能率属于这 4 个层级的某一个层级之下，而这个 4 个层级本身也有隶属关系。也就是说对于员工（staff）来说，可能是这样

```ruby
class Post < ApplicationRecord
  belongs_to :group1
  belongs_to :group2
  belongs_to :group3
  belongs_to :group4
end
```

但是，如果隶属了 `:group1`，那就不应该再隶属于其他 group，所以 `:group2` ~ `:group4` 这三个栏位要是空值才行。如何现实呢？加一句 `optional: true` 就行。

```ruby
class Post < ApplicationRecord
  belongs_to :group1, optional:true
  belongs_to :group2, optional:true
  belongs_to :group3, optional:true
  belongs_to :group4, optional:true
end
```

这样，就可以想连哪个就连哪个了。

[上述只是举例，复杂的层级关系应该有更好的解决办法]

---

**参考文献**

[Rails 5 makes belongs_to association required by default](http://blog.bigbinary.com/2016/02/15/rails-5-makes-belong-to-association-required-by-default.html)