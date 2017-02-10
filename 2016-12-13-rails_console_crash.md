---
title: rails_console_crash
categories: fullstack
tags:
  - RoR
---

教程步骤：

> Rails 第三课： Rails 101
> [3-1 建立讨论群的架构](hhttps://fullstack.xinshengdaxue.com/posts/59)

在 step 5，打开`rails console`之后，输入`Group.create(title: "Board 1", description: "Board 1 body")`，rails console 出现崩溃，直接显示长长的错误信息，一直往上翻也翻不到头。终端的显示的行数有限制，看不到错误信息的最开始部分

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1201941/yNrKjzOrR2LC8baieBBA_Screen%20Shot%202016-12-13%20at%205.23.27%20AM.png" alt="Screen Shot 2016-12-13 at 5.23.27 AM.png">

Google 了一下，发现在终端里，`cmd`+`I`可以打开一个 Preferences 的窗口，在 Terminal 的标签页下，可以设置滚屏的行数，直接设置成 Unlimited scrollback。

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1201941/oqjkfW9DRa6X5cXq5EpS_Screen%20Shot%202016-12-13%20at%206.09.51%20AM.png" alt="Screen Shot 2016-12-13 at 6.09.51 AM.png">

回到 terminal，就可以一直往上翻页，一直到错误的最开始。错误这这样的。

```
/Users/anli/.rvm/gems/ruby-2.3.1/gems/activerecord-5.0.0.1/lib/active_record/connection_adapters/sqlite3_adapter.rb:27: [BUG] Segmentation fault at 0x00000000000110
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-darwin16]
```

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1201941/CTVTRtX4RRWDUI77t6jn_Screen%20Shot%202016-12-13%20at%206.10.05%20AM.png" alt="Screen Shot 2016-12-13 at 6.10.05 AM.png">

于是按照提示，在`~/Library/Logs/DiagnosticReports`里找到了 crash report，但是完全看不懂。在 Slack 上提问等待老师解决。

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1201941/IRd9alvfSlmQ3m829Hfk_Screen%20Shot%202016-12-13%20at%206.12.27%20AM.png" alt="Screen Shot 2016-12-13 at 6.12.27 AM.png">

练习被迫中止，做不下去了。

Google 了一下，在 StackExchange 上找到一个相同问题（[链接](http://stackoverflow.com/questions/39969734/rails-5-bug-segmentation-fault)），似乎是sqlite3 gem的一个bug，但好像也没有明确的解决办法。
