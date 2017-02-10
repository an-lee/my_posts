---
title: Session是什么意思
date: 2017-02-07
categories: fullstack
tags: 
  - RoR
  - rails
---

# 问题的提出

> 课程名称：Rails 实战：购物网站
>
> 课程步骤：[Step3: 购物车 Part2](https://fullstack.xinshengdaxue.com/posts/416)

在购物车的实作时，「加入购物车」这个动作实现是在 `application_controller.rb` 添加了以下代码

```ruby
def	current_cart
  @current_cart || = find_cart
end

private

def	find_cart
  cart = Cart.find_by(id: session[:cart_id])
  if cart.blank?
    cart = cart Cart.create
  end
  session[:cart_id] = cart.id
  return cart
end
```

这段代码出现了很多我目前非常陌生的单词，第一个就是 `session`。那 `session` 是什么意思呢？

# 字面意思

编程世界里的代码其实绝大部分都是常用的单词，而其功能也是往往跟单词本身意思相关的。换句话说，大多数的代码其实光从字面意思我们就能理解个七七八八。

session 的意思是 *a period of time that is spent doing a particular activity*， 也就是「用来做某件事情的一段时间」。光从这个意思来看上面的代码，似乎也还是看不出个所以然来。

# 在Rails中的意思

经过 Google，在 [RailsGuides](http://guides.rubyonrails.org) 上找到了详细解释。有两个解释可以帮助我理解

第一个解释是这样的

> Your application has a session for each user in which you can store small amounts of data that will be persisted between requests.
>
> 摘自 [Action Controller Overview](http://guides.rubyonrails.org/action_controller_overview.html#session)

意思是说，`session` 是用来保存某个用户在**两次请求之间**的一些少量数据的。那这些「少量数据」是用来做什么的呢？

另一个解释是这样的

> HHTP is a stateless protocol. Sessions make it stateful.
>
> Most applications need to keep track of certain state of a particular user. This could be the contents of a shopping basket or the user id of the currently logged in user. Without the idea of sessions, the user would have to identify, and probably authenticate, on every request. Rails will create a new session automatically if a new user accesses the application. It will load an existing session if the user has already used the application.
>
> 摘自 [Ruby on Rails Security Guide](http://guides.rubyonrails.org/security.html#sessions)

读到这里，其实可以回想其上一课 JobListing 里也见过 `session` 的出现，是在创建用户系统的 `devise` 相关代码里。

HTTP 是一个没有「状态」概念的协议。在 JobListing 那一课以及 JDStore 这一课里，我们都用 `devise` 这个 Gem 创建了一个用户系统，然后创建了一个管理员的用户。后台的页面是需要经过管理员验证才可以进入的，用了 `before_action :authenticate_user!` 和 `before_action :admin_required` 来过滤，也就是说首先要是会员，然后还得是管理员，才可以进入这个后台页面。

当我们以游客的身份想要登录后台页面时，系统会将页面转向登录页面。然后我们以管理员身份登录之后，就可以返回到后台页面了。而当我们在后台再一次操作任何一个动作时，因为有着过滤系统，系统需要再一次验证到底是不是管理员在操作。而这个时候，如果没有 `session` 这个功能的话，我们只能再一次返回到登录界面，再一次登录。换句话说，**我们在后台页面的每一个操作都需要重新登录管理员来验证一遍**。显然这是不符合我们的使用习惯。

这就是所谓「HTTP是一个没有“状态”概念的协议」。虽然在同一个页面（后台页面）上操作的始终是我一个人，但是网站系统不知道这几个操作一直都是我发出的，它需要一遍又一遍地进行验证。而 `session` 就是用来告诉系统，在登录之后、登出之前，网页是处于「管理员登录」的状态，就不要一遍又一遍地来验证了。

# Session的工作原理

通过阅读 [RailsGuides](http://guides.rubyonrails.org) 上的解释，根据我的理解，session 的工作原理可能是这样的。

当我们登录一个页面时，`session` 系统为每一个登录网页的用户分别记录了一个「状态」，并且为此创建了一个 `session_id`。而这个 `session_id` 是保存在用户浏览器本地的 cookies 里的。这样，网站通过读取我们浏览器 cookies 中的 `session_id` 就能知道当下是处于一个什么状态了。

在用户系统里，当我们登录之后，这个登录状态的信息就被 `session` 记录下来了，`session_id` 保存在浏览器的 cookies 里。网站通过读取 `session_id` 就能确定现在是「管理员登录」状态，就不会每一个操作都跑来要求登录了。

在购物车系统里其实也是一个道理，虽然还没有登录，但其实 `session` 已经为每一个用户创建了一个「购物车状态」，当购物车里新增东西时，这个状态就会被更新，只要浏览器里的 cookies 没有被清除，`session_id` 还在，那这个「状态」就得到保存，购物车里的商品也会被保存。

# 其他概念

`session_id` 里包含一个 `hash value`，也就是**哈希值**，好像是一种加密方法，可以简单地理解为「[指纹](https://zh.wikipedia.org/wiki/%E6%95%A3%E5%88%97%E5%87%BD%E6%95%B8)」。

`session` 是和 `cookies` 一起工作的，关于` cookies`，维基百科上有这样的[解释](https://zh.wikipedia.org/wiki/Cookie)

> 因为[HTTP协议](https://zh.wikipedia.org/wiki/HTTP)是无状态的，即服务器不知道用户上一次做了什么，这严重阻碍了[交互式Web应用程序](https://zh.wikipedia.org/w/index.php?title=%E4%BA%A4%E4%BA%92%E5%BC%8FWeb%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F&action=edit&redlink=1)的实现。在典型的网上购物场景中，用户浏览了几个页面，买了一盒饼干和两饮料。最后结帐时，由于HTTP的无状态性，不通过额外的手段，服务器并不知道用户到底买了什么。 所以Cookie就是用来绕开HTTP的无状态性的“额外手段”之一。服务器可以设置或读取Cookies中包含信息，借此维护用户跟服务器会话中的状态。
>
> 在刚才的购物场景中，当用户选购了第一项商品，服务器在向用户发送网页的同时，还发送了一段Cookie，记录着那项商品的信息。当用户访问另一个页面，浏览器会把Cookie发送给服务器，于是服务器知道他之前选购了什么。用户继续选购饮料，服务器就在原来那段Cookie里追加新的商品信息。结帐时，服务器读取发送来的Cookie就行了。
>
> Cookie另一个典型的应用是当登录一个网站时，网站往往会请求用户输入用户名和密码，并且用户可以勾选“下次自动登录”。如果勾选了，那么下次访问同一网站时，用户会发现没输入用户名和密码就已经登录了。这正是因为前一次登录时，服务器发送了包含登录凭据（用户名加密码的某种加密形式）的Cookie到用户的硬盘上。第二次登录时，（如果该Cookie尚未到期）浏览器会发送该Cookie，服务器验证凭据，于是不必输入用户名和密码就让用户登录了。

