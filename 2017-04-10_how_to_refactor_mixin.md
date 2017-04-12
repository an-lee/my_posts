---
title: 重构套路3_mixin
date: 2017-04-10 08:30
categories: fullstack
tags:
  - Rails
  - refactor
---

# 什么是 mixin

先看从网上找来的一个[例子](https://www.tutorialspoint.com/ruby/ruby_modules.htm)

```ruby
module A
   def a1
     #stuff
   end
   def a2
     #stuff
   end
end
```

```ruby
module B
   def b1
     #stuff
   end
   def b2
     #stuff
   end
end
```

```ruby
class Sample
include A
include B
   def s1
     #stuff
   end
end
```

在这个例子里，在 module A 里定义两个 method，分别是 `a1` 和 `a2`，在 module B 里也定义两个 method，分别是 `b1` 和 `b2` ，在 Sample 这个 class 里也定义了一个 method，即 `s1`。注意到，在 Sample 还有两句代码，即

```ruby
include A
include B
```

这样的结果是，对于 Sample 这个类，我们可以除了可以使用在自己本身定义的那个 method `s1` 以外，也可以使用 `a1`，`a2`，`b1`，`b2` 这些 method 了。也就是说，下面这些使用都是成立的

```ruby
samp = Sample.new
samp.a1
samp.a2
samp.b1
samp.b2
samp.s1
```

这样就是所谓的 mixin。

# mixin 和 ActiveSupport::Concern

还是看具体的例子，在购物网站中的订单，即 Order 这个 model，需要用 `generate_token` 这个 method

```ruby
class Order < ActiveRecord::Base
  before_create :generate_token
  def generate_token
    self.token = SecureRandom.uuid
  end
end
```

如果有其他 model 也要用到 `generate_token` 这个 method 的时候，我们自然就想到 mixin，把这个 method 分离出来，放到一个 module 里面去，然后再用 `include` 来引用。

一般是在 app/models/concerns 下新建一个 module ，比如说 `tokenable.rb`

```ruby
module Tokenable
  def generate_token
    self.token = SecureRandom.uuid
  end
end
```

然后，Oder 这个 model 就可以改成

```ruby
class Order < ActiveRecord::Base
  include Tokenable
end
```

但是，要注意到，原来的 Oder 这个 model 里还有一个细节，就是 `before_create :generate_token` ，也就是说，每次新建 order 时都要先运行 `generate_token ` 这个 method。现在这个 method 已经搬到 app/models/concerns/tokenable.rb 里面去了。而问题是，在 Ruby 里，纯 module 是不认识 `before_create` 的，Rails 内部对此有一个解决办法，就是 `ActiveSupport::Concern`。将 module 改成这样

```ruby
module Tokenable

  extend ActiveSupport::Concern
  
  included do 
    before_create :generate_token 
  end
  
  def generate_token
    self.token = SecureRandom.uuid
  end
end
```

问题就解决了。

# 总结

1. 遇到需要被多个 model 使用的 method 时，用 mixin 把这个 method 写到 module 里去；
2. 如果这个 method 需要支持类似 `before_create` 这样的 callback，要记得用 `ActiveSupport::Concern`。