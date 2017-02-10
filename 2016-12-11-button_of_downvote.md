---
title: '加分题：扣分按钮的实现'
date: 2016-12-08 05:41
categories: [fullstack]
tags:
  - RoR
---

教程步骤：
> Rails 第二课：初级练习
> [3-13 加分题 & 下一步](https://fullstack.xinshengdaxue.com/posts/39)

教材前面示范了增加“加分”按钮，而增加“扣分按钮”要做的事情跟加分按钮相反。

回顾之前的教材步骤，投票功能的实现主要有3步，分别是

> [3-7 对Topic投票](https://fullstack.xinshengdaxue.com/posts/46)
> [3-8 把投票记录和 Topics 接起来](https://fullstack.xinshengdaxue.com/posts/47)
> [3-9 让大家可以投票](https://fullstack.xinshengdaxue.com/posts/48)

而 upvote（即加分） 的功能是在[3-9 让大家可以投票](https://fullstack.xinshengdaxue.com/posts/48)这一节里实现的。仿效这一节，可以实现“扣分”功能。

# 目标

加一个按钮，让大家可以按下去就投票减分。

# 步骤

## 步骤1：加一个新的 controller action 来投票减分

编辑`app/controllers/topics_controller.rb`，在 keyword `private`之上新增一个 downvote 的 method。

```ruby app/controllers/topics_controller.rb
def downvote
  @topic = Topic.find(params[:id])
  @topic.votes.first.destroy
  redirect_to(topics_path)
end
```

注意：upvote的 method 是`topic.votes.create`，而downvote 的 method 不是`topic.votes.destroy`，而是`topic.votes.first.destroy`。这个讯息是从[3-8 把投票记录和 Topics 接起来](https://fullstack.xinshengdaxue.com/posts/47)中找到的。

实践证明`topic.votes.destroy`无法实现删除投票。

## 步骤2：给投票减分操作加一个 route

打开 `config/routes.rb`，找到原来的这个：

```ruby
Rails.application.routes.draw do
 root 'topics#index'
 resources :topics do
   member do
     post 'upvote'
   end
 end
end
```

增加一个 downvote 的 route，代码变成这样：

```ruby
Rails.application.routes.draw do
  root 'topics#index'
  resources :topics do
    member do
      post 'upvote'
    end
    member do
      post 'downvote'
    end
  end
end
```

检查 route 有没有成功加入，在 terminal 里输入`rake routes`，输入结果里应该多`downvote_topic`这一行，整个输出结果像这样

```
Prefix Verb   URI Pattern                    Controller#Action
  root GET    /                              topics#index
upvote_topic POST   /topics/:id/upvote(.:format)   topics#upvote
downvote_topic POST   /topics/:id/downvote(.:format) topics#downvote
topics GET    /topics(.:format)              topics#index
       POST   /topics(.:format)              topics#create
new_topic GET    /topics/new(.:format)          topics#new
edit_topic GET    /topics/:id/edit(.:format)     topics#edit
 topic GET    /topics/:id(.:format)          topics#show
       PATCH  /topics/:id(.:format)          topics#update
       PUT    /topics/:id(.:format)          topics#update
       DELETE /topics/:id(.:format)          topics#destroy
```

## 步骤3：在 view 里面加一个按钮

编辑`app/views/topics/index.html.erb`最后一个 loop ， 在 `button_to ‘+1’`的下一面新增一行`button_to '-1'`，修改结果如下

```html
<% @topics.each do |topic| %>
  <tr>
    <td><%= link_to topic.title, topic %></td>
    <td><%= pluralize(topic.votes.count, "vote") %></td>
    <td><%= button_to '+1', upvote_topic_path(topic), method: :post %></td>
    <td><%= button_to '-1', downvote_topic_path(topic), method: :post %></td>
    <td><%= link_to 'Delete', topic, method: :delete, data: { confirm: 'Are you sure?' } %></td>
  </tr>
<% end %>
```

## 步骤4：在浏览器里面检查修改成功

在 `rails server`的情况下，打开 [http://localhost:3000/topics](http://localhost:3000/topics)，在`+1`按钮后面多了一个`-1`。
