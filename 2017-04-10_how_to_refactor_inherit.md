---
title: 重构套路2_继承
date: 2017-04-10 08:00
categories: fullstack
tags:
  - Rails
  - refactor
---

在购物网站的后台里，可能会出现几个 controller 都出现一些同样的代码，比如说后台的订单(order)和后台的商品(Product)，都需要设置权限和套用后台的 layout。

```ruby
class Admin::ProductsController < ApplicationController
  before_action :authenticate_user!
  before_action :admin_required

  layout "admin"
  
end
```

```ruby
class Admin::OrdersController < ApplicationController
  before_action :authenticate_user!
  before_action :admin_required

  layout "admin"
  
end
```

看到重复的代码，要想起 DRY 原则。如何来重构呢？可以用「继承」这个特性。

新建一个 `AdminController` ，按照后台的情况设置好通用的特性

```ruby
class AdminController < ApplicationController
  before_action :authenticate_user!
  before_action :admin_required

  layout "admin"

end
```

然后，对于后台里的其他 Controller，就可以直接 `继承` 这个 `AdminController` 的特性（都需要注册才可以登入，都限定 admin 才可以登入，都使用 admin 这个 layout）。

```ruby
class Admin::ProductsController < AdminController
end
```

```ruby
class Admin::ProductsController < AdminController
end
```

代码第一句里的 `<` 就是 `继承` 的意思。

如果再去观察其他一般的 controller，都是 默认继承于 `ApplicationController`，而再去看 `ApplicationController`，又继承于 `ActionController::Base` 。

```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception
end
```

而 `ActionController::Base` 是什么呢？引用一段[说明](http://api.rubyonrails.org/classes/ActionController/Base.html)，先了解一下。

> Action Controllers are the core of a web request in Rails. They are made up of one or more actions that are executed on request and then either it renders a template or redirects to another action. An action is defined as a public method on the controller, which will automatically be made accessible to the web-server through Rails Routes.
>
> By default, only the ApplicationController in a Rails application inherits from `ActionController::Base`. All other controllers in turn inherit from ApplicationController. This gives you one class to configure things such as request forgery protection and filtering of sensitive request parameters.