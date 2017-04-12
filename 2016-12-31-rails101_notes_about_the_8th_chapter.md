---
title: Rails101第8章笔记
date: 2016-12-31
categories: fullstack
tags:
  - Rails
---

Rails 101已经完成了3遍，现在开始第四遍。这一遍开始，把自己的理解纪录下来。

本章的目标是实现可以查看个人发表的文章和参加过的社团。

---

## 8-2 产生 account 的 namespace 下的 GroupsController

### 步骤

1.`rails g controller account/groups`
2.修改 confit/routes.rb，增加

```ruby
namespace :account do
  resources :groups
end
```

### 我的理解

目标是看某个用户名下的所有参加过的 groups，首先想到的是这个页面需要一个路径，合理的路径应该是 .../account/groups，这是一个单独的页面。在 app/views 下，已经有的文件夹有 /common、/groups、/layout、/posts、/welcome，用户的页面无处安放。所以新建一个控制器 account/groups，随之就有了一个 views, views/account/groups了。

再添加路径，用的是`namespace`，不知道是什么意思。

`rake routes`之后看增加的路径有这些

```ruby
account_groups GET    /account/groups(.:format)                  account/groups#index
               POST   /account/groups(.:format)                  account/groups#create
new_account_group GET    /account/groups/new(.:format)              account/groups#new
edit_account_group GET    /account/groups/:id/edit(.:format)         account/groups#edit
 account_group GET    /account/groups/:id(.:format)              account/groups#show
               PATCH  /account/groups/:id(.:format)              account/groups#update
               PUT    /account/groups/:id(.:format)              account/groups#update
               DELETE /account/groups/:id(.:format)              account/groups#destroy
```

在 /account/groups 里也可以有 new、edit 这些 action，但是对应的是同一个 model 吗？

---

## 8-2 account/groups_controller.rb 下的 index action

### 步骤

```ruby
def index
  @groups = current_user.participated_groups
end
```

### 我的理解

在 /controllers/account 下的 groups_controller.rb 里用的 `current_user` 和 `participated_groups`，用法跟 /controllers下的 groups_controller.rb 是一样的，连结的是同一个model。

不知道这个连结是怎么建立起来的？是跟 routes 里的`namespace`有关吗？还是说，只要是名称相同，不论在哪个层级下连结的都会是同一个 model？我猜可能是后者。

---

## 8-3 额外作业

增加对 Post 的“编辑”和“删除功能”，这个额外作业在上一次做的时候已经成功实现，这次再重新做的时候，[竟然出现了错误](http://an-lee.pro/posts/2016/12/31/1259996)，找了好久原因，才找到。

### 我的理解

在 /app/controllers/account 下与 /app/controllers 下有两个名字一模一样的 controllers，要注意各自的 action 是不同的。在 /controllers/account 下的 controllers 只有一个 index 的 action，实现的是在用户页面下显示自己所发表过的文章和参加过的讨论版。而 /controllers 下的两个 controllers 则是对 post 和 group 这两个 model 的直接操作。
