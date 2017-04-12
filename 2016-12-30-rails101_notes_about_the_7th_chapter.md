---
title: Rails101第7章笔记
date: 2016-12-30
categories: fullstack
tags:
  - Rails
---

Rails 101已经完成了3遍，现在开始第四遍。这一遍开始，把自己的理解纪录下来。

本章的目标是建立群成员和 group 的关系，并限制发文章的权限。

---

## 7-2 捞出“参与的所有群”

### 步骤

在`app/models/group_relationships.rb`里加入两行

```ruby
belongs_to :group
belongs_to :user
```

然后在`app/models/user.rb`里加入两行

```ruby
has_many :group_relationships
has_many :participated_groups, :through => :group_relationships, :source => :group
```

### 我的理解

之前在`group`这个 model 里加了一栏`user_id`，是用来记录 group 的创建者，每个 group 的创建者只有一个，每个用户可以创建多个群组。group 与 user 之间是一对多的关系，所以在 group 里多增加一栏即可清晰记录。

而现在，除了创建者，group 群内还可以有很多群组成员，每个用户又可以加入到多个群，这样群与群成员之间的关系就不是一对一的关系，而是多对多的关系。为了简便记录，就需要建立一张独立的关系表来将关系捋清。这张关系表只有两列，大概是这样的

group_id | user_id
:---:| :---:
 1|1
 1|2
 2|1
 2|3
 3|2
 ...|...

为了找出某个用户参与的所有群组，比如说用户1，那就在这张表里，找出所有满足 user_id=1 这个条件的 group_id。上述这张表里，user_id=1 时的group 包括1和2。

上述的代码中，`participated_groups`其实是一个自定义的变量，后面的代码即是该变量的定义：

> 在`group_relationships`这张表里，user 栏为当下用户的所有 group

---

## 7-2 设定 Group 与 User 之间的关系

### 步骤

在`app/models/group.rb`里增加两行

```ruby
has_many :group_relationships
has_many :members, through: :group_relationships, source: :user
```

### 我的理解

总体思路跟上一步的捞出用户参与所有群组的大致一样。但是，是对比具体语句

> 找出用户参与的所有群组

```ruby
has_many :participated_groups :through => :group_relationships, :source => :group
```

> 找出某群组的所有的成员

```ruby
has_many :participated_groups through: :group_relationships, source: :user
```

经过验证，`:through =>`等效于`through:`，`:source =>`等效于`source:`。

从代码的直观性来看，前者虽然啰嗦点，但是更好理解。

---

## 7-3 是否“成员一份子”的判断式

### 步骤

在`app/models/user.rb`里加入判断函数

```ruby
def is_member_of(group)
  participated_groups.include?(group)
end
```

### 我的理解

有了这个判断函数，我们就可以用`user.is_member_of(group)`这个句式来判断一个用户是否属于某个群组了。细看这个函数，其实很简单，用到了一个应该是内建的函数`include?`。

如果不写这个判断函数，应该也可以直接用`user.participated_groups.include?(group)`这个句式来到达同样的效果。但这明显显得太啰嗦了。

还有就是`is_member_of?`为什么要在末尾加一个问号，教材上说“暂时把它视为' method 名字的一部分','暂时'没有意义”。换句话说，它是有意义的，后面再说。

---
## 7-4 加入群组或退出群组

### 步骤

在`app/models/user.rb`里定义两个函数

```ruby
def join!(group)
  participated_groups << group
end
def quit!(group)
  participated_groups.delete(group)
end
```

### 我的理解

这里的的惊叹号，教材同样说“暂时把它视为' method 名字的一部分','暂时'没有意义”。

这两段代码不是很好理解，`participated_groups`是自定义的，返回的值应该是「用户参与的所有群组」，应该是一个 group_id。

在`quit!`里,`participated_groups.delete(group)`可以理解成把包含着当前的 user_id 和返回的 group_id 这条记录在关系表中删除。

在`join!`中`participated_groups << group`可以理解成，把当前的 user_id 和返回的 group_id 作为一条新记录增加到关系表里。

---

## 7-5 加入群组和退出群组

### 步骤

1. 在`groups_controller.rb`里新建`join`和`quit`两个 action
2. 在`routes.rb`里新增`join`和`quit`两个路径
3. 修改`show.html.erb`正确显示

### 我的理解

在前面的操作中，已经在`user.rb`中加入了`join!`和`quit!`这两个函数，但这是在 model 里函数，其动作是与另一个 model `group_relationship`的数据库进行交换传输数据，

根据 MCV 的基本功能划分，在实际操作中，必须由 Controller 来判断是否执行某个动作，所以要将这个数据库层级的修改动作定义在 Controller 里。这个可能解释了为什么在`user.rb`里要定义成`join!`,后面的感叹号可能就是用来区分其与 Controller 里的`join`的。

routes 里增加的语句是

```ruby
resources :groups do
(+)  member do
(+)    post :join
(+)    post :quit
(+)  end
  resources :posts
end
```

修改之后，`rake routes`出来的路径多了两列

```
join_group POST   /groups/:id/join(.:format)                 groups#join
quit_group POST   /groups/:id/quit(.:format)                 groups#quit
```

不懂，先记着。
