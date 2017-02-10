---
title: localhost无法打开
date: 2016-12-10 07:04
categories: [fullstack]
tags:
  - RoR
---

教程步骤：
> Rails 第二课：初级练习
> 3-8 把投票纪录和 Topics 接起来

进入 Rails console，在terminal输入

```ruby
rails c
```

出现错误，如下图

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1189062/d1TGPbRKKRbQS3pYSwhw_Screen%20Shot%202016-12-10%20at%206.54.13%20AM.png" alt="Screen Shot 2016-12-10 at 6.54.13 AM.png">

尝试运行

```ruby
rails server
```

发现 [http://localhost:3000](http://localhost:3000) 无法打开页面。

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1189062/6TqaOx5xR9KAf5CYB2vB_Snip20161210_3.png" alt="Snip20161210_3.png">

重复了之前的几个步骤，包括

```ruby
rails generate model vote topi_id:integer
```

提示冲突，若有强制替换，加上 --force，于是输入

```ruby
rails generate model vote topi_id:integer --force
```

替换成功，接着输入

```ruby
rake db:migrate
```

提示

```
== 20161209223923 CreateVotes: migrating ======================================
-- create_table(:votes)
rake aborted!
StandardError: An error has occurred, this and all later migrations canceled:
```

再试

```ruby
rails c
```

仍旧出现上述错误。

回想之前做过的事情，在3-6步骤时，我把程式上传到 Heroku 时，打开了全局VPN。

**把 VPN 关掉**， [http://localhost:3000](http://localhost:3000) 页面可以打开了。但**出现了错误**。

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1189062/vx3u8bxMRmS0HGBJ3NEs_Snip20161210_4.png" alt="Snip20161210_4.png">

放弃，重来。回到3-1重新开始。
