---
title: group和groups
categories: fullstack
tags:
  - RoR
---

教程步骤：

> Rails 第三课： Rails 101 #第二次
> [4-2 实作讨论群“新增”功能](https://fullstack.xinshengdaxue.com/posts/62)

第二次不能复制代码，要纯手打。在抄代码的时候，很容易「出戏」，手在抄，脑子不知道飘到哪里去了。然后就抄出了很多 Syntax Error。

在4-2的 Step2里，抄完代码之后，刷新网页，出来一列 Syntax Error。检查起来很费劲。

![Screen Shot 2016-12-17 at 6.39.23 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1214540/LJ1G6rQgSp2XqcWP1H0E_Screen%20Shot%202016-12-17%20at%206.39.23%20AM.png)

例如，我把`<%=`打成了`<% =`，多一个空格，看了好几遍才看出来。

把 Syntax Error 都修改完了，刷新网页，还是有错，不是 Syntax Error 的错了。

![Screen Shot 2016-12-17 at 7.01.30 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1214540/K0H8m4K1SeWbb3hdGD0C_Screen%20Shot%202016-12-17%20at%207.01.30%20AM.png)

4-2才做了两个 step，不是 step 2出错就是 step 1出错。step 1的内容是在`app/controller/groups_controller.rb`里加入`new` action。

```ruby app/controller/groups_controller.rb
def new
  @group = Group.new
end
```

这么一点代码也能打错？我反复看了好几遍，才发现我把`@group`打错成`@groups`了。

![Screen Shot 2016-12-17 at 7.12.06 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1214540/ZPq4fd5IRtSpog8QHYoA_Screen%20Shot%202016-12-17%20at%207.12.06%20AM.png)

实际上，在上一个`index` action 里用的是就是`@groups`。这两者有啥不同呢？

我猜，`@groups`是复数，指代多个 group，在`index`里是多个 group 的列表显示，所以用复数。而`@group`是单数，当需要对单个 group 进行操作时使用。新增 group 的`new` action 就是对单个 group 进行操作的。
