---
title: '加分题：新增About页面'
categories: [fullstack]
tags:
  - RoR
---

教程步骤：
> Rails 第二课：初级练习
> [3-13 加分题 & 下一步](https://fullstack.xinshengdaxue.com/posts/39)

最后一道加分题题目如下：
> 新增一个 'about' 页面，并将连结放在 topics 列表的最下方。记得也要在 About 页面上放置回 Topics 列表的连结，以免使用者迷路。

到目前为止，所有的操作围绕着一个东西，即**Topic**，这个 Topic 是在[3-4 建立 Migration](https://fullstack.xinshengdaxue.com/posts/43)中用下面的命令生成的

```ruby
rails generate scaffold topic title:string description:text
```

后来新增的投票功能都是在 Topic 上的修改。而现在是要**新增**一个页面，这个页面叫 About ，按照我的理解，应该是与之前的 Topic 无关的。所以，可能应该需要用的`rails generate`命令来生成一个。

无从下手，Google 之。找到一个 YouTube 上的教学视频 [Create your first pages (about page & contact page) - Ruby on Rails Tutorial](https://www.youtube.com/watch?v=nc3uyXdOuRY)，学到一个语句`rails generate static_pages`，问题就解决了。

# 目标

> 1.新增一个 'about' 页面，并将连结放在 topics 列表的最下方。
> 2.在 About 页面上放置回 Topics 列表的连结

# 步骤

## 步骤1：新增'about'页面

在 terminal 中输入

```ruby
rails generate staitc_pages about
```

输出结果

```
create  app/controllers/static_pages_controller.rb
route  get 'static_pages/about'
invoke  erb
create    app/views/static_pages
create    app/views/static_pages/about.html.erb
invoke  test_unit
create    test/controllers/static_pages_controller_test.rb
invoke  helper
create    app/helpers/static_pages_helper.rb
invoke    test_unit
invoke  assets
invoke    coffee
create      app/assets/javascripts/static_pages.coffee
invoke    scss
create      app/assets/stylesheets/static_pages.scss
```

可以发现，在`app/controllers/`、`app/views/`、`app/assets`和`app/helper`中都生成了一些文件。其中`app/views/static_pages/about.html.erb`是我要的**About**页面。

在 Atom 里打开这个文件，是一个 html 页面，默认有一个`<h1></h1>`和一个`<p></p>`。修改成想要的文字。

```html
<h1>About</h1>
<p>The first step to Fullstack.</p>
```

## 步骤2：将'about'的超链接放在 topics 列表的最下方

在 terminal 输入`rake routes`，可以发现比之前多了一行

```
static_pages_about GET    /static_pages/about(.:format)  static_pages#about
```

打开`/app/views/topics/index.html.erb`，参照上面的代码，可以推测'about'页面的地址可以表示为`static_pages_about_path`。于是，在最下方增加两句

```html
<br>
<%= link_to 'About', static_pages_about_path %>
```

其中`<br>`是换行的意思。

打开[http://localhost:3000/topics](http://localhost:3000/topics)，可以看见新增了'About'的链接。

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1196505/R3jLsdyQTZeQL11cQpzN_Screen%20Shot%202016-12-11%20at%2010.32.34%20AM.png" alt="Screen Shot 2016-12-11 at 10.32.34 AM.png">

## 步骤3：在'about'页面中增加返回Topics列表的链接

在`app/views/static_pages/about.html.erb`中最后增加

```html
<br>
<%= link_to 'Topics', topics_path %>
```

打开[http://localhost:3000/topics](http://localhost:3000/topics)检查一下。

<img class="center" src="http://user-image.logdown.io/user/22009/blog/21058/post/1196505/xMqQJRj7Sfy60aeeTF8x_Screen%20Shot%202016-12-11%20at%2010.32.46%20AM.png" alt="Screen Shot 2016-12-11 at 10.32.46 AM.png">

大功告成。
