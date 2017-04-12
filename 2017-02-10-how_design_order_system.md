---
title: 订单系统的设计思路
date: 2017-02-11 08:01
categories: fullstack
tags: 
  - cheatsheet
  - Rails
---

# 建立结帐页

根据 MVC 的架构，无论是实现什么功能，步骤都可以分为四步，那就是创建网址(Routing)、创建交互（View）、创建数据模型（Model）和创建控制器（Controller）。按照这个思路，把教程的步骤整理一下。

## Routing

第一步，就是给结帐页面创建一个网址，所有网址都在 `route.rb` 里管理

```ruby
resource :carts do
  collection do
    delete :clean
(+)   post :checkout
  end
en
```

回顾一下 Rails 里的 **RESTful 概念**。HTTP 的动作分为四种，分别是 GET, POST, PUT, DELETE。它们跟 CRUD 的关系是

| GET  | POST   | PUT/PATCH | DELETE  |
| ---- | ------ | --------- | ------- |
| 读取   | 新增     | 更新        | 删除      |
| Read | Create | Update    | Destroy |

当我们在 `route.rb` 里用 `resources` 时，默认创建对某个 model 的 CRUD 网址，其中的 action 包括

- index
- create
- new
- show
- update
- destroy
- edit

在这里，我们要做的动作是「结帐」，即 checkout ，这个动作不属于 CRUD 里的任何一个 action，所以要自定义。又因为这其实是一个「向HTTP发出请求」（Request）的动作，所以将其定义为 `post`。

增加`post :checkout` 这行代码之后，就产生了一个网址，`http://yoursite.com/carts/checkout` ，用 `checkout_carts_path` 来调用，其 method 是 post。

## Model

我们的目的是创建一个订单来结帐，那么就必需一个记录订单的数据库，也就是需要创建一个 model，取名 order

```
ralis g model order
```

这个数据库里需要记录哪些信息呢？一般来说，至少包括

- 订单总价
- 创建订单的用户
- 购买人的姓名
- 购买人的地址
- 收件人的姓名
- 收件人的地址

创建的 model 之后，其实只是在 `db/migrate/` 下创建了一个包含创建命令的文件，即`xxxxx_create_orders.rb`，有了这个文件，再运行 `rake da:migrate` ，才会真正在数据库里创建一个新的表单。我们可以通过修改`xxxxx_create_orders.rb`，来对这个表单进行控制。

按照需要，这个表单需要包含上述的信息，所以

```ruby
class CreateOrders < ActiveRecord::Migration[5.0]
  def change
    create_table :orders do |t|
+      t.integer :total, default: 0
+      t.integer :user_id
+      t.string :billing_name
+      t.string :billing_address
+      t.string :shipping_name
+      t.string :shipping_address
      t.timestamps
    end
  end
end
```

然后，运行

```
rake db:migrate
```

订单的数据库就创建完成了。

## View

既然有了地址，有了数据库，接下来，我们要为用户创建一个页面，以便用户填写所需要的信息，然后发送到数据库里保存起来。所以，现在要做的是在 views 里创建一个页面。

```
touch app/views/carts/checkout.html.erb
```

编辑这个页面，根据 HTML 的格式，把这个页面写出来。其中发送表单信息，要用到 `simple_form_for` 这个 Gem。

## Controller

到现在为止，万事俱备，但是还差一个控制器，即 Controller，也是 View 和 Model 之间的连接。我们要让服务器知道，当用户在页面（View）上点击「创建订单」这个按钮时，就把用户填写的信息发送到数据库里保存起来。Controller 的作用就是这个指挥官。

创建一个专属于 order 这个 model 的 controller

```
rails g controller orders
```

然后编辑 `orders_controller.rb` ，在里面设计相关的动作，基本动作就是 CRUD。

创建订单，其实就是在表单中新建一行数据，所以，我们要定义的是 `create` 这个动作

```ruby
def create
  @order = Order.new(order_params)
  @order.user = current_user
  @order.total = current_cart.total_price

  if @order.save
    redirect_to order_path(@order)
  else
    render 'carts/checkout'
  end
end

private

def order_params
  params.require(:order).permit(:billing_name, :billing_address, :shipping_name, :shipping_address)
end
```

在这里，用户点击「创建订单」后，服务器把请求发送到控制器，控制器识别请求之后，把信息保存到数据库中。这些动作完成之后，代码里有一句 `redirect_to order_path(@order)`，意思是跳转到刚刚创建好的这张订单的页面。但是至此， 这个页面还没有存在，那是下一个步骤要做的事情。

# 建立购买明细

当顾客决定购买时，作为商城，应该给顾客打印一张订单，正常来讲，订单的信息之间保存在 oder 这个数据库里就完了。

但值得注意的是，在创建订单的时候，订单里有关商品的信息是直接调用 Product 的数据，但是某一个商品在将来可能会下架或者、价格变动，显然在打印订单的时候，应该把当下的商品和价格记录下来。

所以要把商品的名称和商品的价格从当下的 product 里搬运出来，单独保存。这样的话，将来 Product 的任何变动都不会影响那些过去的订单信息了。

这里，新建一个 model 去保存这些信息，这个 model 记录的信息最好要包括

- 订单编号
- 商品名称
- 商品数量
- 订单总价

##  Model

创建 ProductList model 来保存订单信息

```ruby
rails g model product_list
```

设定 model 的表单

```ruby
class CreateProductLists < ActiveRecord::Migration[5.0]
  def change
    create_table :product_lists do |t|
      t.integer :order_id
      t.string  :product_name
      t.integer :product_price
      t.integer :quantity
      t.timestamps
    end
  end
end
```

### 建立 model 间的联系

新建的 ProductList model 记录的是订单的一部分信息，应该隶属于 Order model，所以对于 `order.rb`

```ruby
has_many :product_lists
```

对于 `productlist.rb`

```ruby
belongs_to :order
```

## Controller

在创建订单过程把订单信息保存到 ProductList 里，要在 controller 中实现。在 `orders_controller.rb` 中

```ruby
  def create
    @order = Order.new(order_params)
    @order.user = current_user
    @order.total = current_cart.total_price

    if @order.save

      current_cart.cart_items.each do |cart_item|
        product_list = ProductList.new
        product_list.order = @order
        product_list.product_name = cart_item.product.title
        product_list.product_price = cart_item.product.price
        product_list.quantity = cart_item.quantity
        product_list.save
      end

      redirect_to order_path(@order)
    else
      render 'carts/checkout'
    end
  end
```

同时，创建完订单之后，还要把订单显示出来，也就是要在 controller 里创建 show 这个 action

```ruby
def show
  @order = Order.find(params[:id])
  @product_lists = @order.product_lists
end
```

## Views

新建订单信息页面 `/views/orders/show.html.erb` ，把订单信息显示出来

其中订单信息部分调用 ProductList 的数据

```erb
    <h2> 订单明细 </h2>

    <table class="table table-bordered">
      <thead>
        <tr>
          <th width="80%">商品明细</th>
          <th>单价</th>
        </tr>
      </thead>
      <tbody>

        <% @product_lists.each do |product_list| %>
          <tr>
            <td>
              <%= product_list.product_name %>
            </td>
            <td>
              <%= product_list.product_price %>
            </td>
          </tr>
        <% end %>

      </tbody>
    </table>
```

至此，购买明细的功能就完成了。

# 将网址改为秘密

创建的订单，其 oder.id 是按顺序依次生成的，是一个流水号，某订单页面的网址是 `http://yoursite.com/orders/:id`，这样一来，其实就把商城的订单量暴露了。我们不希望这样，应该把网址中的订单号隐藏起来。办法是用一个随机生成的乱码来代替订单号。

## Model

每一个订单号对应一个乱码，也就是要在 Order model 表单中新建一列，用来储存乱码。

```ruby
rails g migration add_token_to_order
```

token 的中文意思是「代币，代金券，象征」等。

然后执行 `rake db:migrate` ，Order 表单就修改完成了。

这个乱码必须是一个没有规律的编码，每一次创建订单的之前，就生成一个。Ruby 内建了一个随机生成器，`SecureRandom.uuid` 。在 `order.rb` 中增加代码

```ruby
class Order < ApplicationRecord
  before_create :generate_token

  def generate_token
    self.token = SecureRandom.uuid
  end
end
```

其中 `before_create` 与之前用的 `before_action` 类似，但不尽相同， `before_action` 后的条件是所有动作执行前都会激活，而 `before_create` 则是只有 create 这个动作执行前才会激活。

## Controller

生成了乱码，接着要做的就是把网址中的订单数用乱码来替代。

将 `/controllers/orders_controller.rb` 中的

```ruby
redirect_to order_path(@order)
```

改成

```ruby
redirect_to order_path(@order.token)
```

将

```ruby
@order = Order.find(params[:id])
```

改成

```ruby
@order = Order.find_by_token(params[:id])
```

至此，订单网址就隐藏起来了。