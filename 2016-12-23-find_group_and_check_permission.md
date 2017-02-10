---
title: find_group_and_check_permission
categories: fullstack
tags:
  - RoR
---

教程步骤：

> Rails 第三课： Rails 101
> [5-5 只有群组的“创始者”可以“修改”“删除”群组资讯](https://fullstack.xinshengdaxue.com/posts/74)

为了实现群组「创始者」的独有权限，在`edit`、`update`和`destroy`这几个 action 里增加当前用户的判断语句，而这些语句都是相同的，为了简化代码，将这些语句封装成一个function，然后就可以多次调用了。
封装的这个 function 叫`find_group_and_check_permission`，把重复的语句复制进去，变成这个样子


却发现出错了。


看错误信息，是`@group.user`的问题。再看这个 function ，对比之前在 action 中的代码，是缺少了对@group的定义，所以代码应该改成这样。

```ruby
def find_group_and_check_permission
  @group = Group.find(params[:id])
  if current_user != @group.user
    redirect_to root_path, alert: "You have no permission."
  end
end
```

这就对了。需要将某段重复代码封装的时候，要注意需要用到的变量有没有定义，因为是独立的代码，需要重新定义其中的变量。
