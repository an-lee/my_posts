---
title: Rails101第2章笔记
date: 2016-12-25
categories: fullstack
tags:
  - Rails
---

Rails 101已经完成了3遍，现在开始第四遍。这一遍开始，把自己的理解纪录下来。

---

## 2-2 建立新专案

### 步骤
```ruby
rails new rails101
```

### 我的理解
创建了一个名为 rails101 的网站，所有文件保存在一个名为 rails101 的文件夹里。打开这个文件夹，能里面的基本组成，文件夹里有

- app
- bin
- config
- db
- lib
- log
- public
- test
- tmp
- vendor

在整个 Rails 101 的练习当中，主要的操作在`app`、`config`和`db`这几个文件夹里，其中又主要是集中在`app`里。`app`里的文件夹包括

- assets
- channels
- controllers
- helpers
- jobs
- mailers
- models
- views

`models`、`views`和`controllers`就是所谓的**MVC**。

它们之间的关系大概是这样的，用户通过浏览器打开一个网站链接时，首先触发的是`controllers`，Rails会检查并使用`controllers`里的某些`action`，这些`action`会去呼叫`models`，`models`就会去读写资料库，把读到的资料回传给`controllers`，然后`controllers`把这些资料扔给`views`，最后`views`把资料经过包装，呈现在浏览器上，就是用户看到的页面样子。

简单来说，可以这么理解，

- Model 负责数据
- View 负责呈现
- controller 负责做决定

`app/config`里的`routes.rb`负责管理网站的访问路径，也是经常使用。

`app/db`里存的就是数据库，每次新建的数据库都存在这里，新建后要`rake db:migrate`。

在主文件夹外面，`Gemfile`负责管理需要的用到的`gem`，每次修改都要`bundle install`。

---

## 2-3 挂 bootstrap 这个 gem

### 步骤

在`Gemfile`里增加`bootstrap-sass`，然后`bundle install`。再将`app/assets/stylesheets/application.css`改名为`application.scss`，在里面增加

```
@import "bootstrap-sprockets";
@import "bootstrap";
```

### 我的理解

`bootstrap-sass`是一个 CSS 的框架，封装了很多可以快速利用的设计元素，比如按钮、下拉选单、表格等。使用方法在其[主页](https://github.com/twbs/bootstrap-sass#a-ruby-on-rails)上有方法。

---

## 2-4 套用 bootstrap

### 步骤

1. 新建`app/views/common`文件夹
2. 新建`_navbar.html.erb`、`_footer.html.erb`文件
3. 修改`app/views/layout/application.html.erb`的 HTML 样式
4. 新建 welcome 页面并把主页指向它

### 我的理解

在[getboostrap.com](http://getbootstrap.com/css/#sass)上有关于`bootstrap`的详细使用说明，在 Rails101 中用到几个组件，包括`navbar`, `container`, `btn`, `dropdown`, `text-center`等，都能找到详细的说明，每一类都有不同的样式，使用时只要用相应的名字调用就可以了。`bootstrap`真的像一个种类极其丰富的网站衣帽间，要穿什么拿什么。详细的组件说明在[这里](http://getbootstrap.com/components/)。

---

## 2-4 新建 controller

### 步骤

```
rails g controller welcome
```

### 我的理解

用`rails g controller controller_name`新建一个`controller`，输入命令后，新建了一系列文件，主要是在`controller`,`helpers`,`test/controller`,`assets`等文件夹下的。其中`views`下新建了一个`welcome`的文件夹，但是空的，需要手工增加，在这里就添加了`index.html.erb`作为欢迎页面。

```
create  app/controllers/welcome_controller.rb
invoke  erb
create    app/views/welcome
invoke  test_unit
create    test/controllers/welcome_controller_test.rb
invoke  helper
create    app/helpers/welcome_helper.rb
invoke    test_unit
invoke  assets
invoke    coffee
create      app/assets/javascripts/welcome.coffee
invoke    scss
create      app/assets/stylesheets/welcome.scss
```

在后面还用到`rails g model`的命令，可以猜想`rails g`是一个新建的命令，可以新建`controllers`和`model`，`views`会随之生成。

---

## 2-5 用flash提示信息

### 步骤

1. 调用bootstrap的 js 组件
2. 增加`_flashes` partial
3. 增加`flashes_helper`
4. 把 falshes 的 partial 放在布局里面去
5. 在需要的地方随时调用`flash`action

### 我的理解

Helpers 是用 ruby 写的"view装饰的小方法"，我猜就是专门拿来装饰的小函数(function)。2-5的逻辑可能是这样的，`flashes_helper`定义了一个装饰的小函数，因为在不同情况该怎么调用这个小函数的代码写成了一个 partial，也就是`_flashes`，在 view 下合适的地方放置这个 partial，然后在 controller 里控制什么时候触发这个 action。
