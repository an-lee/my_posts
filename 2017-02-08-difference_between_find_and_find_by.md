---
title: find和find_by的区别
date: 2017-02-08 20:00
categories: fullstack
tags:
  - cheatsheet
  - homework
---

在[这里](http://api.rubyonrails.org/classes/ActiveRecord/FinderMethods.html)找到答案。

对于 `find` 的解释是这样的

> Find by id - This can either be a specific id (1), a list of ids (1, 5, 6), or an array of ids ([5, 6, 10]). If one or more records can not be found for the requested ids, then [RecordNotFound](http://api.rubyonrails.org/classes/ActiveRecord/RecordNotFound.html) will be raised. If the primary key is an integer, find by id coerces its arguments using `to_i`.

对于 `find_by` 的解释是这样的

> Finds the first record matching the specified conditions. There is no implied ordering so if order matters, you should specify it yourself.
>
> If no record is found, returns `nil`.

换言之，这两个 method 最大的不同点在于，当找不到满足条件的值时，**两者的返回值不同**。`find` 会返回一个 *RecodrNotFound* 的错误，而 `find_by` 即使找不到满足条件的值也不会报错，仅仅是返回一个空值 *nill* 。

在本教程中， `find` 其实在 controller 的 CRUD 里就经常用到，例如

```ruby
@product = Product.find(params[:id])
```

在这种情况，如果 `find` 找不到满足条件的值，程序是无法进行下去的，所以要报错，以便让用户知道。

而对于 `find_by` 的相关代码如下

```ruby
def find_cart
  cart = Cart.find_by(id: session[:cart_id])
  if cart.blank?
    cart = Cart.create
  end
  session[:cart_id] = cart.id
  return cart
end
```

在这里，cart_id 是不一定存在的，所以即使 `find_by` 找不到满足条件的 cart_id，只要返回一个空值让我们知道就行了。紧跟着就把如果返回的值是空值，即 `if cart.blank?` 这种情况的处理方法加进去。