---
title: 购物车的设计思路
date: 2017-02-08
categories: fullstack
tags:
  - Rails
typora-copy-images-to: ipic
---

# 第1步：创建加入购物车的 action

## 1.1 新增add_to_cart的网址

要有加入购物车这个 action，首先得有一个发出指令的网址。而网址的管理是集中在 `config/routes.rb` 里，所以在这里添加

```ruby
resources :products
    member do
      post :add_to_cart
    end
end
```

这样一来，就会增加一个网址 `http://yoursite/product/id/add_to_cart`，可以通过 `add_to_cart_path(product_id)` 来引用。

## 1.2 新增add_to_cart的action

这个网址发出的指令，其实是指向 `products_controller`  的一个 action，也就是 `products#add_to_cart`，所以要在`products_controller.rb` 中增加这个 action，以便让服务器知道当接收到发来的网址请求时，这个 action 如何来操作。所以在 ` products_controller.rb` 中新增

```ruby
def add_to_cart
  @product = Product.find(params[:id])
  redirect_to :back
  flash[:notice] = "test for cart"
end
```

这只是创建了 `add_to_cart` 这个 action，使得 `http://yoursite/product/id/add_to_cart` 这个网址有了实际指向。但只是有了一个框框而已，「添加到购物车」这个动作其实还没实现。

# 第2步：实现加入购物车的action

## 2.1 创建购物车和商品的model

回顾一下 Ruby on Rails 中的的架构 `MVC` 。

> - Model 负责数据
> - View 负责呈现
> - Controller 负责做决定

我们现在要实现的是，用户可以把自己喜欢的**商品**添加到自己的**购物车**内。从网站系统的角度来考虑，这里涉及到四个数据，也就是四个Model，分别是

> - 用户：我需要知道是哪个用户在购物
> - 商品：都有哪些商品可以放到购物车里
> - 购物车：每一个用户都得有专属的一辆购物车，否则会乱掉
> - 被放到购物车内的商品：我需要知道哪些商品被添加到了购物车内

因为此前已经用 devise 这个 Gem 创建了用户系统，也就是用户这个 model 已经创建好了。商品的Model，也就是 product 在前面的步骤也已经创建好了。

所以，这里先创建购物车和购物车内商品这两个 model，分别是 `cart` 和 `cart_item`。

```
rails g model cart
rails g model cart_item
```

## 2.2 购物车和商品的关系

好的购物体验应该是这样的：每一个用户都有一辆专属的购物车，每一辆购物车里可以增加无限个商品。

区别于现实中的超市，对于网上商城来说，当我们说把商品放入购物车的时候，其实我们**并没有**把那件商品从货架上拿了下来，而只是在把那件商品记了下来。我想起在宜家的购物流程，可能可以作为类比。

当我们逛宜家的时候，我们的购物流程大概是这样的，如果在展区看到一件心仪的家具，那我们会拿一张宜家提供的购物清单表，把那件心仪家具的货号记在自己的购物清单表上，然后继续逛。再遇到心仪的家具时，依然是把它的货号等信息记在清单表上。逛完之后，我们根据清单表上的货号、货架、位置等信息去提货区提货，最后再到收银台买单。

![宜家的购物](http://okgqgpbx3.bkt.clouddn.com/blog/2017-02-08-122250.jpg)

在我们的购物网站里，我们的购物车 `cart` 其实就相当于宜家购物里的那张购物清单表，所有进店的顾客都可以随手拿一张；购物网站里的商品 `product` 就相当于宜家商城展区所陈列的商品样板；而购物车里的商品，即 `cart_item` 就相当于在宜家购物时写在购物清单表上的那些商品货号。

所以对于购物车 `cart` 来说

```ruby
has_many :cart_items
has_many :products, through: :cart_items, source: :product
```

`cart_item` 就是放在购物车的商品货号，从属于购物车的，所以第一句很好理解。

但是，`cart_item` 仅仅是一个商品货号而已，当顾客逛完之后，决定要买单时，单单有货号还不行，还得替顾客根据货号把真正的商品取出来，放到购物车里。第二句代码的意思就是，根据货号(`through: :cart_items`)从仓库里把商品(`source: :product`)取出来放进购物车里。

对于购物车里的商品 `cart_item` 来说

```ruby
belongs_to :cart
belongs_to :product
```

## 2.3 分发一辆购物车

正常来讲，如果是商城的会员，应该有专属的购物车，为会员长期保存心仪的商品，让会员随时可以购买。（这部分功能后面的教程应该会实现）

但是作为商城，打开门做生意，凡是进店的顾客，不管是不是会员，都应该先为顾客把购物车准备好，让顾客随时可以购物。

游客，也就是非会员的购物车是不需要长期保存在服务器的数据库里的，只需要在游客浏览的过程暂时保存。

给顾客分发购物车的代码如下：

```ruby
def current_cart
  @current_cart ||= find_cart
end

private

def find_cart
   cart = Cart.find_by(id: session[:cart_id])
   if cart.blank?
     cart = Cart.create
   end
   session[:cart_id] = cart.id
   return cart
 end
```

代码的逻辑应该是这样的：

- 第(1)步：顾客进店（用户登录网站）
- 第(2)步：观察顾客是否已经推着购物车（`@current_cart ||= find_cart`）
- 第(3)步：如果没有，就在他的浏览器里找找看有没有之前用过的购物车（`session[:cart_id]`）
- 第(4)步：如果有，就把他原有的购物车编号找出来记着(`cart = Cart.find_by(id: session[:cart_id])`)
- 第(5)步：如果没有(`if cart.blank?`)，那就拿一部新的购物车交给用户(`cart = Cart.create`)，并把购物车编号记着
- 第(6)步：把顾客的购物车编号记在session里
- 第(7)步：完成，顾客手里已经推着购物车了

## 2.4 把商品添加到购物车

当顾客推着购物车在逛了，我们还要帮助顾客把看中的商品放进自己的购物车里。相应于宜家购物的类比，这一步其实是相当于，把心仪商品的货号抄在购物清单上。

实现的代码是这样的：

```ruby
def add_product_to_cart(product)
  ci = cart_items.build
  ci.product = product
  ci.quantity = 1
  ci.save
end
```

这段代码怎么理解呢？添加购物车的步骤分为4步，对应着4行代码，其意思分别是：

- 第(1)步：在购物清单里新建一行
- 第(2)步：填上所添加商品的id号
- 第(3)步：填上添加商品的数量
- 第(4)步：把购物清单保存

这里的 `ci` 只是个变量的名称，应该是取 Cart_Item 的意思。变量名称只是为了阅读方便，对代码执行一般没什么影响。