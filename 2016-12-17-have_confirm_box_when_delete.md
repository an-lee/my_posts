---
title: 删除时没有弹窗确认
date: 2016-12-17
categories: fullstack
tags:
  - Rails
---

教程步骤：

> Rails 第三课： Rails 101 #第二次
> [4-5 实作讨论群“删除”功能](https://fullstack.xinshengdaxue.com/posts/65#task)

4-5完成后，成功增加了「删除」功能，但教程上的图片显示，点击删除时，应该有一个弹窗确认「Are you sure？」

![FnxZhWa8RmqCAdo1pFSb_螢幕快照 2016-07-11 下午2.54.31.png](http://user-image.logdown.io/user/22009/blog/21058/post/1216941/1JKdeWqyRQaiR32G4NHF_FnxZhWa8RmqCAdo1pFSb_%E8%9E%A2%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-11%20%E4%B8%8B%E5%8D%882.54.31.png)

但我的却没这个效果，直接就删除掉了。

![Screen Shot 2016-12-17 at 12.12.36 PM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1216941/i2xttTlIRaKkQcA8tzlu_Screen%20Shot%202016-12-17%20at%2012.12.36%20PM.png)

翻看`app/views/groups/index.html.erb`的代码，

```ruby
<tbody>
  <% @groups.each do |group| %>
  <tr>
    <td>#</td>
    <td><%= link_to(group.title, group_path(group)) %></td>
    <td><%= group.description %></td>
    <td>
      <%= link_to("Edit", edit_group_path(group), class: "btn btn-sm btn-default")%>
      <%= link_to("Delete", group_path(group), class: "btn btn-sm btn-default", method: :delete, data: {conform: "Are you sure?" } )%>
    </td>
  </tr>
  <% end %>
</tbody>
```
确实有`{conform: "Are you sure?" }`这一句，不知道为什么没起效果。而且，这里面的`method`用的是`delete`，`groups_controller.rb`里的写的明明是`destroy`。

不理解，先记着，回头再来回答。
