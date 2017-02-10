---
title: navbar显示不正常
categories: fullstack
tags:
  - RoR
---

教程步骤：

> Rails 第三课： Rails 101
> [5-1 让这个网站有实际“登入”、“登出”的功能](https://fullstack.xinshengdaxue.com/posts/72)

在 step2 ，修改完 `_navbar.html.erb`和`application.js`之后，刷新网页，出来的效果是这样的，“注册”和“登入”分成了两行显示。

![Screen Shot 2016-12-23 at 5.56.04 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1236174/aZxsSLmDR5CmlYdutw2N_Screen%20Shot%202016-12-23%20at%205.56.04%20AM.png)

我反复检查了 step1 里修改的代码，没有错。还是找不到原因，我打开了github上之前做第二遍的代码，逐行对照，发现在前面有个`class`里名称打错了-_-!!!

![Snip20161223_3.png](http://user-image.logdown.io/user/22009/blog/21058/post/1236174/xrZCWfImQWifLUmjyo0I_Snip20161223_3.png)

应该是
```ruby
<ul class ="nav navbar-nav navbar-right">
```

修改之后，马上变正常。

![Snip20161223_4.png](http://user-image.logdown.io/user/22009/blog/21058/post/1236174/X6xIyxzXRRGOit1lACWc_Snip20161223_4.png)

这个地方是在前面就打错了，但错误一直没反映出来。算是前面埋了坑，到这一步给踩了。
