---
title: Post的“编辑”、”删除“功能
categories: fullstack
tags:
  - RoR
---

教程步骤：
> Rails 第三课：Rails 101
> [8-3 可以看到自己发表的所有文章](https://fullstack.xinshengdaxue.com/posts/88)

本章的末尾有一道额外作业，现在是已经是第三次练习，尝试做一下。

题目是：
> 练习自己实作“编辑”和“删除”按钮这两个功能

`Edit`和`Delete`这两个按钮已经实现了，现在要做的工作是把它们链接到实际的编辑和删除动作。

### Step 1 在 posts_controller 里增加 `edit`、`update` 和 `destroy` 等 action

在`app/controllers/posts_controller.rb`增加三个 action

![Screen Shot 2016-12-24 at 11.43.42 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1240713/gJxf93eVTgmAifk6HAIp_Screen%20Shot%202016-12-24%20at%2011.43.42%20AM.png)

```ruby app/controllers/posts_controller.rb
def edit
  @group = Group.find(params[:group_id])
@post = Post.find([:id])
end

def update
  @group = Group.find(params[:group_id])
  @post = Post.find([:id])
  if @post.update(post_params)
    redirect_to account_posts_path, notice: 'Post Update Success'
  else
    render :edit
end

def destroy
  @group = Group.find(params[:group_id])
  @post = Post.find([:id])
  @post.destroy
  redirect_to account_posts_path, alert: 'Post deleted'
end
```

### Step 2 新建 `edit.html.erb`

输入
```
touch app/views/posts/edit.html.erb
```

打开`edit.html.erb`，在里面输入

```html
<div class="col-md-4 col-md-offset-4">
  <h2>编辑文章</h2>
  <hr>
  <%= render "form" %>
</div>
```

这段代码其实是跟`app/views/groups/edit.html.erb`一样，只是把标题名字改了。这里同样用带了 Parital `_form`，所在要在该文件夹下新建一个`_form.html.erb`以供调用。

输入
```
touch app/views/posts/_form.html.erb
```

打开`_form.html.erb`，同样把`app/views/groups/_form.html.erb`里的代码拷贝过来，修改成这样


![Snip20161224_5.png](http://user-image.logdown.io/user/22009/blog/21058/post/1240713/89IzZHGNScyCnnA9GseA_Snip20161224_5.png)

```ruby
<%= simple_form_for [@group, @post] do |f| %>

<div class="form-post">
  <%= f.input :content, input_html: {class: "form-control"} %>
</div>
<%= f.submit "Submit", class:"btn btn-primary", data: {disable_with: "Submitting..."} %>
<% end %>
```

保存。尝试一下，成功了。

![Screen Shot 2016-12-24 at 11.42.02 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1240713/Fsnh5tT4ySvQ70sxAdxA_Screen%20Shot%202016-12-24%20at%2011.42.02%20AM.png)


![Screen Shot 2016-12-24 at 11.42.13 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1240713/Sr4aILdQiK5vchatmbQL_Screen%20Shot%202016-12-24%20at%2011.42.13%20AM.png)

这里要注意，Posts 里的`_form.html.erb`里第一句代码，是用`[@group, @post]`，如果用的是`@post`，就会发生以下错误。

![Screen Shot 2016-12-24 at 11.39.00 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1240713/Zxq0VDmTI0CDSR7aelRA_Screen%20Shot%202016-12-24%20at%2011.39.00%20AM.png)

我是怎么知道的？在`app/posts/new.html.erb`里有相同的语句，依葫芦画瓢。
