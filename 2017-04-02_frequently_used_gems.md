---
title: 常用gems总结
date: 2017-04-02 07:00
categories: fullstack
tags:
  - Rails
  - cheatsheet
---

# Bootstrap

Github 地址: https://github.com/twbs/bootstrap-sass

修改 gemfile 。

```ruby
gem 'bootstrap-sass'
```

修改 `app/assets/stylesheets/application.css` 为 `app/assets/stylesheets/application.scss`，并加入

```scss
@import "bootstrap-sprockets";
@import "bootstrap";
```

在 `app/assets/javascripts/application.js`加入

```javascript
//= require bootstrap-sprockets
```

# simple_form

Github 地址：https://github.com/plataformatec/simple_form

修改 gemfile

```ruby
gem 'simple_form'
```

安装 bootstrap 样式

```
rails generate simple_form:install --bootstrap
```

使用示例

```html
<%= simple_form_for @user do |f| %>
  <%= f.input :username, label: 'Your username please', error: 'Username is mandatory, please specify one' %>
  <%= f.input :password, hint: 'No special characters.' %>
  <%= f.input :email, placeholder: 'user@domain.com' %>
  <%= f.input :remember_me, inline_label: 'Yes, remember me' %>
  <%= f.button :submit %>
<% end %>
```

# devise

Github 地址：https://github.com/plataformatec/devise

修改 gemfile

```ruby
gem 'devise'
```

安装

```
rails generate devise:install
```

创建用户 model

```
rails generate devise user
rake db:migrate
```

 注册/登录/登出入口

```html
<% if !current_user %>
	<li><%= link_to("注册", new_user_registration_path) %> </li>
	<li><%= link_to("登入", new_user_session_path) %></li>
<% else %>
	<li class="dropdown">
		<a href="#" class="dropdown-toggle" data-toggle="dropdown">
          Hi!,<%= current_user.email %>
			<b class="caret"></b>
		</a>
		<ul class="dropdown-menu">
			<li> <%= link_to("登出", destroy_user_session_path, method: :delete) %> </li>
		</ul>
	</li>
<% end %>
```

