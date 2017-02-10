---
title: rails_g之后长时间无反应
categories: fullstack
tags:
  - RoR
---

教程步骤：

> Rails 第三课： Rails 101
> [3-1 建立讨论群的架构](hhttps://fullstack.xinshengdaxue.com/posts/59)

在 step 1 ，terminal 中输入

```ruby
rails g model group title:string description:text
```

预期情况是，运行结束后终端回到命令行输入状态。但是，终端却长时间无反应，就像死机了一样。昨天做这一步的时候同样遇到了这个问题。

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1201907/S0SVhz9fSKywsvfX8nSj_Screen%20Shot%202016-12-13%20at%205.04.43%20AM.png" alt="Screen Shot 2016-12-13 at 5.04.43 AM.png">

等了很久，按`Ctrl`+`C`退出了命令。在 Atom 中考可以发现，新的`app/models`、`/db`和`/test`这几个文件夹里都有新文件生成。

将 VPN 设置为全局，重新输入`rails g model group title:string description:text`命令，运行结果是

```
Running via Spring preloader in process 2199
Expected string default value for '--jbuilder'; got true (boolean)
      invoke  active_record
   identical    db/migrate/20161212210431_create_groups.rb
   identical    app/models/group.rb
      invoke    test_unit
   identical      test/models/group_test.rb
   identical      test/fixtures/groups.yml
```

这次返回到了命令输入状态。

在下一步输入`rails g controller groups`又出现了一样的情况，这个时候 VPN 是开着全局的。

不知道什么原因，依旧是用`Ctrl`+`C`退出，继续往下走。
