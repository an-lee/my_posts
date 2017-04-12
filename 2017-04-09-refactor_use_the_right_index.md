---
title: 后端提效套路3_使用高效的index
date: 2017-04-09 08:30
categories: fullstack
tags:
  - Rails
  - refactor
typora-copy-images-to: ipic
---

当我们要找某一笔资料的时候，最好是要[靠索引](http://an-lee.pro/2017/04/08/2017-04-08-refactor_abandon_table_scan/)（`index`），但是索引和索引也是不一样的，它们之间有着效率高低的差别。

效率的高低取决于用于索引的这个栏位的类型。比如说以下这张图：

![FB30FFF1-3323-414B-809F-29AFB4BE5C62](http://okgqgpbx3.bkt.clouddn.com/blog/2017-04-09-FB30FFF1-3323-414B-809F-29AFB4BE5C62.png)

换句话说，用布尔值来做索引是最快的，因为需要判断的值只有两种，即「真」和「假」。用 DateTime 来索引是最慢的，因为需要判断的东西太多了，年、月、日、星期几、时、分、秒、时区，都要检查一遍才能下判断。

为了提高速度，我们尽可能要用速度快的类型来做索引。比如说，我们要把捞出来的订单按时间顺序来排列，最典型的方法是

```ruby
def index
  @orders = Order.order("created_at DESC")
end
```

也就是用 `:created_at` 这个栏位来做索引。但是这个栏位的格式是 DateTime，效率显然是低下的。

其实，我们可以换一种角度，order 的另一个栏位 `:id` 也是按照创建时间的先后来依次排列的，也就是说，用 `:id` 来排列可以得到同样的效果，但是效率却能大大提高。所以，这种情况，应该改成

```ruby
def index
  @orders = Order.order("id DESC")
end
```



