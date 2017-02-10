---
title: Rails101第6章笔记
categories: fullstack
tags:
  - RoR
---

Rails 101已经完成了3遍，现在开始第四遍。这一遍开始，把自己的理解纪录下来。

这一章的目标是增加发文章的功能。

---

## 6-2 设立 posts 的 routes

### 步骤

将`routes.rb`中修改成

```ruby
resources :groups do
  resources :posts
end
```

### 我的理解

这里的`posts`是嵌套在`groups`里面的，`rake routes`查看路径，`posts`的相关路径有

```
/groups/:group_id/posts
/groups/:group_id/posts/new
/groups/:group_id/posts/:id
```

---

## 6-4 文章倒序排列

### 步骤

1. 在`post.rb`里建一个 scope: `scope :recent, -> {order("created_at DESC")}`
2. 在`groups_controller.rb`的 show 下：`@post = @group.posts.recent`

### 我的理解

在初级练习中有一道加分题，是让 topics 按点赞数来排列，当时 Google 之后，用`sort_by`来实现的，具体语句是：

```ruby
@topics = @topics.sort_by{ |topic| topic.votes.count }.reverse
```

这里又用到`order`，还是没搞明白有什么区别。

另外，用 API Scope 来封装查询功能，是放在 model 里的，这里就是放在`post.rb`里。

---
**updated at 2017-01-02**

这里的 scope 是在 post.rb 下的，而调用的时候是在 groups_controller.rb 里。这个 recent 被写成了 post 的一个「属性」，在其他地方调用时用的是 `post.recnet`，有点像 python 里的用法。
