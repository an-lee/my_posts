---
title: Segmentation_fault再次出现
categories: fullstack
tags:
  - RoR
---

教程步骤：

> Rails 第三课： Rails 101
> [5-4 让“群组”与“使用者”产生关联](https://fullstack.xinshengdaxue.com/posts/73)

用`bundle update`解决了一次`[BUG] Segmentation fault`问题（[见这里](http://an-lee.pro/posts/2016/12/13/1204518)）之后，在5-4的 Step 6中，输入

```ruby
rails c
Group.delete_all
```

再次出现了同样的`[BUG] Segmentation fault`问题。

```
anli@Ans-Mac-mini ⮀ ~/rails101 ⮀ ⭠ ch04± ⮀ rails c
Running via Spring preloader in process 9077
Loading development environment (Rails 5.0.0.1)
2.3.1 :001 > Group.delete_all
/Users/anli/.rvm/gems/ruby-2.3.1/gems/activerecord-5.0.0.1/lib/active_record/connection_adapters/sqlite3_adapter.rb:27: [BUG] Segmentation fault at 0x00000000000110
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-darwin16]
```

StackExchange上[那个帖子里](http://stackoverflow.com/questions/39812707/segmentation-fault-with-rails-after-upgrading-to-os-sierra-possibly-related-to)有人说`bundle update`只是暂时解决问题，果然，又被卡住了。
