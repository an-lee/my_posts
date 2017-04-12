---
title: 后端效能检测_Bullet
date: 2017-04-09 10:00
categories: fullstack
tags:
  - Rails
  - refactor
typora-copy-images-to: ipic
---

[后端的效能瓶颈](http://an-lee.pro/2017/04/08/2017-04-08_how_to_refactor_in_back_end/)多半在于数据库方面，而[数据库的效率低下](http://an-lee.pro/2017/04/08/2017-04-08-refactor_activerecord/)主要问题有

- N+1 Query
- Table Scan
- Counter Cache

# 如何自动检测

在 Rails 代码里需要避免这些问题。那如何检测自己的代码是否存在这些问题呢？面对庞大的代码时，有没有高效的检测方法呢？答案是有的，有个专门检测此类问题的工具，就是 [Bullet](https://github.com/flyerhzm/bullet)。

# 使用方法

## 安装 gem

在 Gemfile.rb 的 `:development` 的 group 中加入

```ruby
gem 'bullet'
```

 然后 `bundle install` 进行安装。

## 设置 bullet

找到 `config/enviroments/development.rb` ，在里面增加以下代码

```ruby
config.after_initialize do
  Bullet.enable = true
  Bullet.alert = true
  Bullet.bullet_logger = true
  Bullet.console = true
  # Bullet.growl = true
  # Bullet.xmpp = { :account  => 'bullets_account@jabber.org',
  #                 :password => 'bullets_password_for_jabber',
  #                 :receiver => 'your_account@jabber.org',
  #                 :show_online_status => true }
  # Bullet.rails_logger = true
  # Bullet.honeybadger = true
  # Bullet.bugsnag = true
  # Bullet.airbrake = true
  # Bullet.rollbar = true
  Bullet.add_footer = true
  # Bullet.stacktrace_includes = [ 'your_gem', 'your_middleware' ]
  # Bullet.stacktrace_excludes = [ 'their_gem', 'their_middleware' ]
  # Bullet.slack = { webhook_url: 'http://some.slack.url', channel: '#default', username: 'notifier' }
end
```

其中注释部分代码是设置 bullet 的通知系统，我们只需要弹窗提示和 footer 提示就好了，所以把其他的都注释掉。如果要用其他 notifier，需要配合其他 notifier 的 gem 才可以使用

## 查看效果

重启 `rails s` ，如果刷新的页面存在上述的三个问题，就会有弹窗提示，并且在网页的左下角有汇总的问题提示。如下图所示，检测表明这个页面有三处 `eager loading` ，也就是出现三个 N+1 Query 的问题。😅

![82CD16AB-CDD8-4F45-8157-453BE524BFAA](http://okgqgpbx3.bkt.clouddn.com/blog/2017-04-09-82CD16AB-CDD8-4F45-8157-453BE524BFAA.png)

![343DD465-52D0-4DF6-82CD-13EE7181A55D](http://okgqgpbx3.bkt.clouddn.com/blog/2017-04-09-343DD465-52D0-4DF6-82CD-13EE7181A55D.png)

![85DD10E8-6B0D-451F-917B-8E9838498737](http://okgqgpbx3.bkt.clouddn.com/blog/2017-04-09-85DD10E8-6B0D-451F-917B-8E9838498737.png)



![79FEBED0-9617-4205-990A-7867C72278DC](http://okgqgpbx3.bkt.clouddn.com/blog/2017-04-09-79FEBED0-9617-4205-990A-7867C72278DC.png)