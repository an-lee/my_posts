---
title: ORID_以需求为导向来学习
date: 2017-04–05 05:30
categories: fullstack
tags:
  - ORID
  - Rails 
---

昨天重构了 controller，将 controller 瘦身，把里面的 methods 搬到了 model，然后用 rspec 写了 model 层级的单元测试，并且测试成功。

新学到了一些豆知识，比如在 Rails 5 以后，采用 `belongs_to` 时，默认会建立 association，关联的那个栏位就会被验证。如果想手动关联，可以用

```ruby
belongs_to :group, optional: true
```

检测 Model 数据的变动来 trigger 一些事件，可以用 Active Record Callbacks，常用的 callbacks 包括

```
# Creating an Object
before_validation
after_validation
before_save
around_save
before_create
around_create
after_create
after_save
after_commit/after_rollback

# Updating an Object
before_validation
after_validation
before_save
around_save
before_update
around_update
after_update
after_save
after_commit/after_rollback

# Destroying an Object
before_destroy
around_destroy
after_destroy
after_commit/after_rollback
```

想让新建数据就触发，可以用 `after_create` ，想让更新数据就触发，可以用 `after_update`。

想追踪 Model 的某一个栏位数据变动，可以用 Active Model Dirty，常用的 method 有 `changed()`，`changed?()`，`previous_changes()`等。还可以用一个 Attribute Method，`_was`，比如 `group_title_was` 就能得到这个 group 更新前的那个 title。

这几天一直比较兴奋，借着做题，学到了非常多的新知识。以需求为导向，这样的学习是最有效率的。

今天继续把 controller 的 spec 做完。