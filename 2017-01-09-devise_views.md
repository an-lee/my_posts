---
title: devise_views
date: 2017-01-09
categories: fullstack
tags:
  - Rails
---

在论坛上学到 devise 的[进一步用法](http://forum.qzy.camp/t/topic/338/2)：

```ruby
rails g devise:views
```

命令输入之后，结果是这样的

```
anli ⮀ ~/job-listing2 ⮀ ⭠ step0± ⮀ rails g devise:views
Expected string default value for '--jbuilder'; got true (boolean)
Expected boolean default value for '--markerb'; got :erb (string)
     invoke  Devise::Generators::SharedViewsGenerator
     create    app/views/devise/shared
     create    app/views/devise/shared/_links.html.erb
     invoke  simple_form_for
     create    app/views/devise/confirmations
     create    app/views/devise/confirmations/new.html.erb
     create    app/views/devise/passwords
     create    app/views/devise/passwords/edit.html.erb
     create    app/views/devise/passwords/new.html.erb
     create    app/views/devise/registrations
     create    app/views/devise/registrations/edit.html.erb
     create    app/views/devise/registrations/new.html.erb
     create    app/views/devise/sessions
     create    app/views/devise/sessions/new.html.erb
     create    app/views/devise/unlocks
     create    app/views/devise/unlocks/new.html.erb
     invoke  erb
     create    app/views/devise/mailer
     create    app/views/devise/mailer/confirmation_instructions.html.erb
     create    app/views/devise/mailer/password_change.html.erb
     create    app/views/devise/mailer/reset_password_instructions.html.erb
     create    app/views/devise/mailer/unlock_instructions.html.erb
```

在`app/views`文件夹下新建了一个`devise`的文件夹，里面的好几个下级文件夹有7个

- confirmations
- mailer
- passwords
- passswords
- registrations
- sessions
- shared
- unlocks

效果就是，注册、登录的界面也套用了`bootstrap`的样式。
