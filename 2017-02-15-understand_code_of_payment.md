---
title: 对订单支付的理解
date: 2017-02-15
categories: fullstack
tags: [RoR]
---

当顾客完成了挑选商品（放到购物车）、决定购买（下订单）这两步时，接下来要做的就是完成支付。支付功能的实现按照 MVC 的思路来实现。

# Routes

首先第一步，还是可以为支付功能创建一个网址。考虑到最常见的支付方式就是支付宝和微信，所以在 order 下新建两个网址，在 `routes.rb` 里增加

```ruby
resources :orders do
  member do
    post :pay_with_alipay
    post :pay_with_wechat
  end
end
```

这样，我们就得到里两个网址，method 是 post。

```
http://yoursite.com/orders/id/pay_with_alipay
http://yoursite.com/orders/id/pay_with_wechat
```

用 `rake routes` 可以查看，调用网址的命令分别是 `pay_with_alipay_order_path` 和 `pay_with_wechat_order_path` 。

如果要增加其他支付方式，比如说信用卡支付，其实可以类似增加

```ruby
post :pay_with_creditcard
```

# Model

model 是用来处理数据的。

## 新建栏位

对于一张订单，作为卖家，需要知道顾客是否已经支付，以及是用什么来支付的。所以在订单 order 这个 model 要增加两个栏位来记录这些信息，栏位起名分别为 is_paid 和 payment_method。

```
rails g migration add_is_paid_to_order
rails g migration add_payment_method_to_order
```

对于 is_paid，其记录的值只有两种可能，即「是」或「否」，所以设定为布尔属性（:boolean）即可，而且默认是未支付状态（default: false）。

```ruby
def change
  add_column :orders, :is_paid, :boolean, default: false
end
```

而对于支付方式却是可以有很多种的，所以 payment_method 这个栏位的信息最好是字符型（:string）的，可以写入支付方式的名称，所以

```ruby
def change
  add_column :orders, :payment_method, :string
end
```

## 新建method

在 Rails 的框架里，有很多内建的 method ，比如 `new`、`create`、`index`、`destroy` 等等，这些 method 都是直接对数据库做更改的。但是没有对应于支付的 method，所以我们要新建 method。

现在起码要新建两个，分别是对 is_paid 和 payment_method 这两个栏位进行修改的动作。因为这两个 method 的范围都是限定在 order 这个 model 里，所以在 `order.rb` 里增加

对于 is_paid

```ruby
def pay!
  self.update_columns(is_paid: true )
end
```

这个 method 起的名字是 `pay!`，既然自建的 method，起名其实是可以随意的，但是为了增强代码的可读性，也尽量要遵循一定的命名规则。

比如说这里的 `!` ，在 Ruby 的规则里，如果内建的 method 名字后面带有 `!`，表示这个 method 会变更自己的状态。什么是「变更自己的状态」，我的理解是，数据库里的数据会被更改并被保存。

我们的这个 method 虽然是我们新增加的，命名最好也遵循相同的规则。`pay!`  这个 method 的执行效果，其实就是把某一个订单的 `is_paid` 栏位的值变更为 `true`，是属于「变更自己的状态」，所以最好命名时带有 `!` ，以便增强代码可读性。

对于 payment_method

```ruby
def set_payment_with!(method)
  self.update_columns(payment_method: method )
end
```

同理，`set_payment_with!` 这个 method 执行的效果是把 `payment_method` 这个栏位的值修改。但与 `is_paid` 这个栏位不筒， `payment_method` 的属性是字符型的，也就是要填入具体的字符。那该填入什么字符呢？最好在执行 method 的时候再来定。

在这一段代码里，`set_payment_with!(method) ` 中，括号里的 `method` 其实就是指代需要写入 `payment_method` 栏位的信息，`method` 也只是一个变量的名称而已。

# Views

接下来，在网页上新建可以点击的按钮，以便顾客点击支付。

在 `/view/orders/show.html.erb` 中增加

```erb
<div class="group pull-right">
    <%= link_to("以支付宝支付", pay_with_alipay_order_path(@order.token), :method => :post, :class => "btn btn-danger") %>
	<%= link_to("以微信支付", pay_with_wechat_order_path(@order.token), :method => :post, :class => "btn btn-danger") %>
</div>
```

这里的按钮，分别链接到了我们在 `routes.rb` 里新建的那两个网址，而且要指定 `method`。

# Controller

当顾客在网页上点击支付的按钮，比如说「以支付宝支付」，可以理解为发起一个「支付」的指令。`routes.rb` 为这个指令起了一个名字，叫做 `pay_with_alipay` ，并且创造了一条通道，即 `pay_with_alipay_order_path`。

那这条通道指向哪里呢？ **指向总控制器 Controller** ，让 Controller 来决定到底要做些什么。

首先，Controller 要识别这个指令，也就是要有相应的 action 才可以对指令进行响应。既然 `routes.rb` 已经起好名字了，那 Controller 要做的，就是创建一个相同的名字 action 进行识别。

那这个 action 要做什么呢？要做两件事。第一、将这张订单的改成「已支付状态」，第二，将这张订单标识为「用支付宝完成支付」。其实就是要把 order 数据库里的两个栏位 `is_paid` 和 `payment_method` 更改。

可是控制器是 **没办法直接修改数据库** 的，那是 model 的权限，所以，controller 的作用是

- 接收 view 传来的指令
- 把指令传给 model
- 把 model 执行后的信息传给 view

所以在 Controller 里的代码是

```ruby
def pay_with_alipay
    @order = Order.find_by_token(params[:id]) 
  	# 识别是哪张订单
  	@order.set_payment_with!("alipay") 
  	# 执行set_payment_with! 这个method,通知model在payment_method栏位填入 alipay
    @order.pay!
  	# 执行pay! 这个method,通知model修改is_paid栏位信息

    redirect_to order_path(@order.token), notice: "使用支付宝成功完成付款"
  	# 成功之后，在网页提示成功执行的信息
  end
```

类似地，再增加一个对应 `pay_with_wechat` 的 action。

```ruby
  def pay_with_wechat
    @order = Order.find_by_token(params[:id])
    @order.set_payment_with!("wechat")
    @order.pay!

    redirect_to order_path(@order.token), notice: "使用微信支付成功完成付款"
  end
```

这样理一遍，好像对 MVC 之间的关系更清楚了。

需要注意的是，在这个步骤中，仅仅是修改了订单的状态，也就是顾客点击「支付」，订单立即修改成了「已支付」状态，其实真正的「支付」过程还没发生。这个支付的功能，我猜应该有相应的 gem 可以之间使用吧？