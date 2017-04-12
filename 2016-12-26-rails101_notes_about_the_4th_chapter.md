---
title: Rails101第4章笔记
date: 2016-12-26
categories: fullstack
tags:
  - Rails
---

Rails 101已经完成了3遍，现在开始第四遍。这一遍开始，把自己的理解纪录下来。

---
## 4-2 new action的表单

### 步骤

在`app/views/groups`下新建`new.html.erb`。

### 我的理解

在`new.html.erb`的代码里用到一个`form_for`，好像是一个内置的函数，还分为`.text_field`和`.text_area`两种，前者是一个固定窗口，后者的窗口则可以放大缩小。

---
## 4-3 定义show action

### 步骤

1. 在`groups_controller.rb`新建`show` action
2. 在 Views 文件夹下新建`show.html.erb`页面

### 我的理解

在控制器`groups_controller`里的`show`动作，其实只做了一件事情，就是定义`@group`为当前所选取的`group`。然后在 Views 里直接调用了`@group`作为当前`group`的显示。

定义`edit` action 时也是类似。

---

## 4-6 限制空标题写入

### 步骤

在`app/models/group.rb`中加入一句`validates :title, presence:true`

### 我的理解
 `validates`是 Rails 内建的「验证」函数。我们的目的是限制标题为空的数据写入数据库，而 Models 是负责与数据交换的，所以是在 Models 里增加验证函数。

---
## 4-7 partial 的运用

### 步骤

新建`_form.html.erb`，放在`app/views/groups`下。把该目录下的`edit.html.erb`和`new.html.erb`里用到的相同的代码拷贝出来放在`_form.html.erb`里。把原来的代码用`<%= render "form" %>`带代替。

### 我的理解

Partial 仅仅是一个段代码，不是一个函数。如果是一个子函数，则必须是「完整」的，而 partial 显然不是这样的，可能仅仅是一段在多个文件中需要重复使用的代码。

Partial 的格式是`_XXX.html.erb`，只能供同一文件夹下其他文件调用。调用的语句是

```
<%= render "XXX" %>
```

这只是缩写的格式，完整表达是

```
<%= render :partial => "XXX" %>
```

用 partial 的好处是，需要多次使用的相同的代码集中在一个文件上，修改起来非常方便。例如练习中就把表格格式改成 Bootstrap 的版型，如果不用 partial，就得在`edit.html.erb`和`new.html.erb`里分别改。用了 partial，只需要在`_form.html.erb`中修改即可。

我经常犯的一个错是，把前面的`<%=`写成`<%`，漏了一个`=`。其实如果后面接的是语句，比如判断语句、重复语句等，就应该用`<%`；而接的是变量，就应该加一个`=`，即

```
<% 语句 %>
<%= 变量 %>
```

---

## RESTful 概念

Helpers | Controllers| | | |
--- | --- | --- | --- |---
HTTP Verb | GET | POST | PUT | DELETE |
groups_path | index | create | |
group_path(group) | show | | update | destroy
edit_group_path(group) | edit | | |
new_group_path | new | | |
