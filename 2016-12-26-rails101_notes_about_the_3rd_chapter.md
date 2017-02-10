---
title: Rails101第3章笔记
categories: fullstack
tags:
  - RoR
---

Rails 101已经完成了3遍，现在开始第四遍。这一遍开始，把自己的理解纪录下来。

---

## 3-1 新建 model

### 步骤

```ruby
rails g model group title:string description:text
rake db:migrate
```

### 我的理解

用`rail g`命令新建了一个 model， 名为`group`，这个 model 有项属性，分别是题目和描述。新建命令完成后，生成了4个文件。

```
invoke  active_record
create    db/migrate/20161225205828_create_groups.rb
create    app/models/group.rb
invoke    test_unit
create      test/models/group_test.rb
create      test/fixtures/groups.yml
```

打开`201612XXXXXXXX_create_groups.rb`这个文件，代码是这样的

```ruby
class CreateGroups < ActiveRecord::Migration[5.0]
  def change
    create_table :groups do |t|
      t.string :title
      t.text :description

      t.timestamps
    end
  end
end
```

这段代码看着像是一段修改数据库的代码。新建完了之后，还要`rake db:migrate`。

*migrate* 这个单词的意思是迁移、转移。`rake db:migrate`之后，发现`app/db`下的`schema.rb`发生了修改，打开看一下

```ruby
ActiveRecord::Schema.define(version: 20161225205828) do

  create_table "groups", force: :cascade do |t|
    t.string   "title"
    t.text     "description"
    t.datetime "created_at",  null: false
    t.datetime "updated_at",  null: false
  end

end
```

在 Finder 里打开该目录，可以发现还有一个文件被修改过，那就是`development.sqlite3`，这个看起来就是数据库。

这个新建的过程可能是这样的，用`rails g`命令只是生成了修改数据库的操作步骤，也就是`201612XXXXXXXX_create_groups.rb`里的代码，然后用`rake db:migrate`来执行`201612XXXXXXXX_create_groups.rb`里修改数据库的命令，发起修改数据库的请求。这时又生成了`Schema.rb`，*schema* 的中文意思是纲要。可能`Schema.rb`才是修改数据库的直接命令。最后才把数据库`development.sqlite3`修改好。

整个过程可能是这样的

`rails g model`新建 >> `201612XXXXXXXX_create_groups.rb` >> `Schema.rg` >> 数据库`development.sqlite3`

---

## 3-1 增加路径

### 步骤

在`config/routes.rb`中增加`resources :groups`

### 我的理解

修改`routes.rb`前，用`rake routes`命令，可以发现此时的路径只有一个，就是欢迎页面。

加入`resources :groups`之后，再`rake routes`，就可以发现增加了很多路径，对应着还有它们支持的 Action。

在需要跳转到某个页面时，可以有 Prefix + `_path`的格式。比如主页就是 `groups_path`，在`index.html.erb`里用到的编辑页面路径是`edit_group_path`，删除页面路径是`group_path`。
