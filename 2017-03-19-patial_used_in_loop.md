---
title: Partial用在循环时
date: 2017-03-19
categories: fullstack
tags:
  - Rails
---

在 Rails 中，使用 Partial 可以使得代码简洁，而且提高效果。将需要重复使用的代码截取出来，放到一个 partial 里，然后就可以接近调用。除了简单地调用，还可以有高阶的用法，例如循环调用。

原代码：

```ruby
<% @groups.each do |group| %>
  <tr>
    <td>#</td>
    <td> <%= link_to(group.title, group_path(group)) %> </td>
    <td> <%= render_group_description(group) %> </td>
    <td> <%= group.user.email %> </td>
    <td>
      <% if current_user && current_user == group.user %>
        <%= link_to("Edit", edit_group_path(group), class: "btn btn-sm btn-default") %>
        <%= link_to("Delete", group_path(group), class: "btn btn-sm btn-default", method: :delete, data: {confirm: "Are you sure?"}) %>
      <% end %>
    </td>
  </tr>
<% end %>
```

这段代码是一段循环代码，对 `@groups` 做了一个 `do` 循环。

```ruby
<% @group.each do |group| %>
...
<% end %>
```

假设我们在很多地方都要用到这段代码，我们可以把它直接扔进一个 partial 里，比如 `_group_item.html.erb`，可以在页面用 `< % render :partial => 'group_item' %>` 来直接调用 。

但是，这样有一个问题，这段代码中的 `@groups` 涉及到页面的 controller 中的 action，如果也放到 partial 里，会使得代码不够清晰，后续维护也变得更困难。所以更明智的做法应该是，把循环内的代码放到 partial，然后使循环调用这个 patial。

新建 partial，名为 `_group_item.html.erb`，代码为

```ruby
<tr>
  <td>#</td>
  <td> <%= link_to(group.title, group_path(group)) %> </td>
  <td> <%= render_group_description(group) %> </td>
  <td> <%= group.user.email %> </td>
  <td>
    <% if current_user && current_user == group.user %>
      <%= link_to("Edit", edit_group_path(group), class: "btn btn-sm btn-default") %>
      <%= link_to("Delete", group_path(group), class: "btn btn-sm btn-default", method: :delete, data: {confirm: "Are you sure?"}) %>
    <% end %>
  </td>
</tr>
```

循环调用的语句为

```ruby
<%= render :partial => "group_item", :collection => @groups, :as => :group %>
```

