---
title: Rails101第9章笔记
date: 2016-12-31
categories: fullstack
tags:
  - Rails
---

Rails 101已经完成了3遍，现在开始第四遍。这一遍开始，把自己的理解纪录下来。

本章的目标是了解一些 helpers 和 partial。

---

## 9-2 simple_format的使用

### 步骤

simple_format(内容)

### 我的理解

HTML 中的换行符号是 `<br>`，不认得系统输入的换行符号 `/r/n`。在 group 的 description 的表格中即使换行了，在网页上也无法显示出来。simple_format 是 Rails 内建的一个 helpers，可以把换行符合 `/r/n` 翻译成 `<br>` ，从而正确显示出来。

同理，在 post 的 content 也应该用 simple_format。

![WX20161231-102147.png](http://user-image.logdown.io/user/22009/blog/21058/post/1260196/mOks1zeSgidj0g9jaX2A_WX20161231-102147.png)

---

## 9-3 自制 helper

### 步骤

在 app/helpers/groups_helper.rb 里增加

```ruby
def render_group_description(group)
  simple_format(group.description)
end
```

然后就可以直接调用 render_group_description 了。

### 我的理解

helper 本质上是一个子函数。

---

## 9-4 partial的用法

### 步骤

1.在 app/views/groups/index.html.erb 里的 @groups.each 内这个循环函数删掉；
2.新建 partial，名为 `_group_item.html.erb`，把循环函数内的代码复制进去，但是去掉循环；
3.在`index.html.erb`调用 partial

### 我的理解

之前我对partial的理解就是一截任意的代码，可以多次重复的调用。在这一节里的调用却不太一样：

```
<%= render :partial => "group_item", :collection => @groups, :as => :group %>
```
在调用的时候还可以用上循环！

对比一下原来的循环语句是 `@groups.each do |group|`，这里的用法是`:collection => @groups, :as => :group`。

不明就里，但感觉 partial 的用途会很大。
