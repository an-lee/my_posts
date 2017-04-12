---
title: edit页面别放错位置了
date: 2016-12-31
categories: fullstack
tags:
  - Rails
---

教程步骤：

> Rails 第三课： Rails 101
> [8-3 可以看到自己发表的所有文章](https://fullstack.xinshengdaxue.com/posts/88)

这一节的额外作业，在做第三遍的时候一次过顺利完成了，但是做第四遍的时候却出现了意想不到的错误。

删除功能可以正常使用，但是“编辑”功能却出现了这个错误：

![Screen Shot 2016-12-31 at 9.12.06 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1259996/I6aU2S3T8auoTUo9sTBc_Screen%20Shot%202016-12-31%20at%209.12.06%20AM.png)

我反复查看代码，甚至翻出第三遍做的代码来比较，**没有错啊**。

翻来覆去也没找到原因，我只好把错误信息 Google 一下，还真的找到了[相同的问题](http://stackoverflow.com/questions/40009571/missing-a-template-for-this-request-format-and-variant)，我看了问题的答案，出现这个错误的原因是，操作的 action 没有对应的 `.html.erb` 文件。

可是，我已经新建了`edit.html.erb` 了啊，代码也没问题，文件位置也没错啊，放在 /views/posts 下面呀。

**哎，不对！**

我把`edit.html.erb`新建在了 /views/account/posts 下面，不是 /views/posts ！

再回去看 /views/account/posts/index.html.erb 这个文件，里面 'Edit' 的是 link_to 到 edit_group_post_path，而不是 edit_account_post_path ！

把 edit.html.erb 的位置挪过去，一切就正常了。

![WX20161231-100412.png](http://user-image.logdown.io/user/22009/blog/21058/post/1259996/3hknVBs7RW68UBpy28fE_WX20161231-100412.png)

不过话说回来，edit_account_post_path 确实是存在的，还有 new_account_post_path 呢，什么情况能用得上呢？
