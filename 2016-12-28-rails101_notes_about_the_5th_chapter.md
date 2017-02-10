---
title: Rails101第5章笔记
categories: fullstack
tags:
  - RoR
---

Rails 101已经完成了3遍，现在开始第四遍。这一遍开始，把自己的理解纪录下来。

这一章的目标是安装会员系统，并设置相关权限。

---
## 5-2 安装会员系统

### 步骤

`rails g devise user`

### 我的理解

在安装完 devise 这个 gem 后，运行命令`rails g devise user`，发现这个命令的效果跟[新建 model](http://an-lee.pro/posts/2016/12/26/1243682)差不多，生产的文件有

```
invoke  active_record
create    db/migrate/20161227214912_devise_create_users.rb
create    app/models/user.rb
invoke    test_unit
create      test/models/user_test.rb
create      test/fixtures/users.yml
insert    app/models/user.rb
route  devise_for :users
```

跟新建 model 不同的是，不需要自定义 model 的栏目了。打开生成的`20161227214912_devise_create_users`，发现会员系统所需要的栏目都设定好了。另外，在`app/models/user.rb`下直接创建了`user.rb`，连路径`routes.rb`也直接修改好，增加了一句`devise_for :users`。

再运行`rake db:migrate`，就在数据库中增加了会员系统。

devise 这个 gem 似乎是专门新建会员系统。因为会员系统的相对包含的信息较多，用`rails g model`来手动定义可能比较麻烦，就有了 devise 这个 gem 来简化工作。而且这里好像只用了 devise 的默认设置，应该还可以自定义一下吧？

还有就是登陆界面也自动完成了。在前面新建 group 这个 model 的时候，每个访问的页面都需要在`app/views/groups`下新建`.html.erb`
的页面。而这次用`rails g devise`新建的会员系统，在`app/views`下没有相关的文件，但却出来了一个登陆页面。这也是 devise 这个 gem 为我们省去的工作。

---

## 5-3 实现登出、登出功能

### 步骤

修改`app/views/common/_navbar.html.erb`

### 我的理解

「注册」、「登入」和「登出」的指向地址分别是`new_user_registration_path`、`new_user_session_path`和`destroy_user_session_path`，这几个地址应该也是 devise 这个 gem 做的好事。

试`rake routes`一下，发现 user 的路径非常丰富

Prefix Verb  |  URI Pattern    |     Controller#Action
--- | --- | ---
new_user_session GET  |  /users/sign_in(.:format)   |    devise/sessions#new
user_session POST  | /users/sign_in(.:format)   |    devise/sessions#create
destroy_user_session | DELETE /users/sign_out(.:format)   |   devise/sessions#destroy
new_user_password GET  |  /users/password/new(.:format) | devise/passwords#new
edit_user_password GET  |  /users/password/edit(.:format) | devise/passwords#edit
user_password PATCH | /users/password(.:format)    |  devise/passwords#update
       PUT  |  /users/password(.:format)   |   devise/passwords#update
       POST  | /users/password(.:format)   |  devise/passwords#create
cancel_user_registration GET  |  /users/cancel(.:format)   |     devise/registrations#cancel
new_user_registration GET  |  /users/sign_up(.:format)   |    devise/registrations#new
edit_user_registration GET  |  /users/edit(.:format)     |     devise/registrations#edit
user_registration PATCH | /users(.:format)       |        devise/registrations#update
       PUT  |  /users(.:format)       |        devise/registrations#update
       DELETE | /users(.:format)      |        devise/registrations#destroy
       POST  | /users(.:format)       |        devise/registrations#create

---

## 5-4 修改数据库

### 步骤

`rails g migration add_user_id_to_group`

修改生成的`20161227223141_add_user_id_to_group.rb`，变成

```ruby
class AddUserIdToGroup < ActiveRecord::Migration[5.0]
  def change
      add_column :groups, :user_id, :integer
  end
end
```

### 我的理解

这一步是为了让数据库原来的 group 增加一个用户号码 user_id 这个属性，以便跟使用者 user 连结起来。

首先用`rails g migration`生成一个修改命令的文件，新生成的`20161227223141_add_user_id_to_group.rb`是这样的

```ruby
class AddUserIdToGroup < ActiveRecord::Migration[5.0]
  def change
  end
end
```

然后其中的`change`函数里增加需要修改的内容，这里要做的就是给 group 增加一个 column，名为 user_id。

---

## 5-4 建立群组和使用者的关联

### 步骤

group `belongs_to` user, user `has_many` groups

### 我的理解

要显示创建者时，调用的变量是`group.user.email`。

---

## 5-5 设置权限

### 步骤

1. 在`app/controllers/groups_controller.rb`的`private`后面增加子函数`find_group_check_permission`；
2. 在`groups_controller.rg`前面增加`before_action :find_group_check_permission, only: [:edit, :update, :destroy]`

### 我的理解

为了限制非会员的游客浏览，用了`:authenticate_user!`，这里再把权限缩小到讨论版的创始者，在`private`下面自定义了一个子函数`find_group_check_permission`。也就是说，`before_action`后面支持内置的函数，也支持自定义的子函数。
