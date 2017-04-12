---
title: class method 和 instance method
date: 2017-04-10 09:30
categories: fullstack
tags:
  - Rails
  - refactor
---

从字面上就能理解，class method 就是对于 class（类） 的方法，instance method 就是对于 instance（实例）的方法。

```ruby
class Test
# 在class内定义method时加上self代表要直接取用class

  def self.class_method
    "class method"
  end

  def instance_method
     "instance method"
  end
end
```

在这个例子里，`self.class_method` 就是类方法，`instance_method` 就是实例方法。

从 Rails 里内建的一些方法能看出这两种不同方法的用法。比如说一个典型的 CRUD controller

```ruby
class GroupsController < ApplicationController
  before_action :authenticate_user! , only: [:new, :create, :eidt, :update, :destroy]
  before_action :find_group_check_permission, only: [:edit, :update, :destroy]

  def index
    @groups = Group.all
  end

  def new
    @group = Group.new
  end

  def show
    @group = Group.find(params[:id])
    @posts = @group.posts.recent.paginate(:page => params[:page], :per_page => 5)
  end

  def edit
  end

  def create
    @group = Group.new(groups_params)
    @group.user = current_user
    if @group.save
      current_user.join!(@group)
      redirect_to groups_path
    else
      render :new
    end
  end

  def update
    if @group.update(groups_params)
      redirect_to groups_path, notice: 'Update Success'
    else
      render :edit
    end
  end

  def destroy
    @group.destroy
    flash[:alert] = 'Group deleted'
    redirect_to groups_path
  end

private

  def find_group_check_permission
    @group = Group.find(params[:id])
    if current_user != @group.user
      redirect_to root_path, alert: "You have no permission"
    end
  end
  def groups_params
    params.require(:group).permit(:title, :description)
  end
end
```

这份 CRUD 里面用到的内建 method 包括 `.all`，`.new`，`.find`，`.save`，`.update`，`.destroy` 等等。

仔细观察一下，对于 `.all` 、 `.new` 和 `.find` 是直接用在 `Group` 这个类后面的，其实就是类方法。

而其他方法，比如说 `.save` ，是要保存一个 group；`.update` 是要更新某一个实例；`.destroy` 是要删除某一个实例，显然是针对实例的。