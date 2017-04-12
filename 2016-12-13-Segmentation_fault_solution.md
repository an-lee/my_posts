---
title: Segmentation_fault的解法
date: 2016-12-13
categories: fullstack
tags:
  - Rails
---

教程步骤：

> Rails 第三课： Rails 101
> [3-1 建立讨论群的架构](https://fullstack.xinshengdaxue.com/posts/59)

在 step 5，打开`rails console`之后，输入`Group.create(title: "Board 1", description: "Board 1 body")`，rails console 出现崩溃，几千行的错误信息。翻到最前面，错误的提示如下：

```
/Users/anli/.rvm/gems/ruby-2.3.1/gems/activerecord-5.0.0.1/lib/active_record/connection_adapters/sqlite3_adapter.rb:27: [BUG] Segmentation fault at 0x00000000000110
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-darwin16]
```

用`Segmentation fault at 0x00000000000110 ruby`作为关键词 Google 出StackExchange的一个同样问题（[点此链接](http://stackoverflow.com/questions/39812707/segmentation-fault-with-rails-after-upgrading-to-os-sierra-possibly-related-to)），似乎是跟 macOS Sierra 有关，我的 mac mini系统版本是10.2.1。仔细看了上面很多人提出的解决办法，都有人回帖说成功解决，也有人说没效果，还有人说只是暂时解决。

不管怎么样，我试了其中一个

```ruby
bundle update
```

然后，就解决了。虽然不知道是不是暂时解决，毕竟可以往下继续了。
