---
title: 重构套路1_before_action
date: 2017-04-10 07:30
categories: fullstack
tags:
  - Rails
  - refactor
---

在典型的 CRUD controller 里，有一句代码会重复出现，就是

```ruby
@group = Group.find(params:[:id])
```

这句代码会在 `show`, `edit`, `update`, `destroy` 里都会出现。碰到重复出现的代码，要想起一个原则，就是 DRY 原则，即：Don't Repeat Yourself。

其实，用 `before_action` 来重构一下，就可以避免这些冗余的代码。比如说

```ruby
class GroupsController < ApplicationController

  before_aciton :find_group, only: [:show, :edit, :update, :destroy]

  def index
    @groups = Group.all
  end

  def new
    @group = Group.new
  end

  def show
  end

  def edit
  end

  def create
    @group = Group.new(group_params)

    if @group.save
      redirect_to groups_path
    else
      render :new
    end
  end

  def update
    if @group.update(group_params)
      redirect_to groups_path, notice: 'Update Success'
    else
      render :edit
    end
  end

  def destroy

    @group.destroy
    redirect_to groups_path, alert: 'Group deleted'
  end

  private



  def group_params
    params.require(:group).permit(:title, :description)
  end

  def find_group
    @group = Group.find(params[:id])
  end
end
```

把重复的代码放到新建的函数 `find_group` 里，然后在开始用 `before_action` 来引用这个函数，再指定适用范围。这样就完成了一次重构。
