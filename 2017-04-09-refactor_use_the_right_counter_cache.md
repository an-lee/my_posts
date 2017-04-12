---
title: 后端提效套路4_使用counter_cathe
date: 2017-04-09 09:30
categories: fullstack
tags:
  - Rails
  - refactor
typora-copy-images-to: ipic
---

当我们需要在 Rails 里计算总共有多少笔资料的时候，比如说计算某一门课有多少学生选修。

对于 Course 来说

```ruby
class Course < ActiveRecord::Base
  has_many :students
end
```

对于 Student 来说

```ruby
class Student < ActiveRecord::Base
  belongs_to :course
end
```

对于 Course Controller

```ruby
class CoursesController < ApplicationController
  def index
    @courses = Course.all
  end
end
```

在 View 的呈现是

```erb
<h1> Courses </h1>
<ol>
  <% @courses.each do |course| %>
  <li><%= link_to course.name, course_path(course) %> (<%= course.students.count %> students )</li>
  <% end %>
</ol>
```

如果我们观察背后的输出，Log 是这样的

```sql
Rendering courses/index
  SQL (0.3ms)   SELECT count(*) AS count_all FROM "students" WHERE ("students".course_id = 61) 
  SQL (0.2ms)   SELECT count(*) AS count_all FROM "students" WHERE ("students".course_id = 62) 
  SQL (0.3ms)   SELECT count(*) AS count_all FROM "students" WHERE ("students".course_id = 63) 
  SQL (0.2ms)   SELECT count(*) AS count_all FROM "students" WHERE ("students".course_id = 64) 
  SQL (0.2ms)   SELECT count(*) AS count_all FROM "students" WHERE ("students".course_id = 65)
```

注意到，这里面用了很多 `count(*)` 。而 这个 `count(*)` 是非常低效的查询方法。所以，为了提效，要尽量避免这种查询方法。那该怎么做呢？

用 counter_cache，也就是在 Rails 里将 count 快速取出来的一种方法。如何来实现呢？

第一步，增加一个栏位

```
rails g migration add_students_count_to_course
```

栏位设置是

```ruby
def change
   add_column :courses, :students_count, :integer, default: 0 
end
```

第二步，修改 model

```ruby
class Student < ActiveRecord::Base
  belongs_to :course, counter_cache: true
end
```

这样，就修改完之后。同样的调取，Log 就会变成这样

```sql
Rendering courses/index
  SQL (0.3ms)   SELECT students_count FROM "courses" WHERE id = 61
  SQL (0.3ms)   SELECT students_count FROM "courses" WHERE id = 62
  SQL (0.3ms)   SELECT students_count FROM "courses" WHERE id = 63
  SQL (0.3ms)   SELECT students_count FROM "courses" WHERE id = 64   
```

这里的原理其实很简单，就是为 Course 增加一个学生数量的栏位 `:student_count` ，每次有学生加入的时候，这门 Course 里的 `:studen_count` 就会实时更新。需要调取总数的时候，直接取 `:student_count` 栏位的数值就可以了，而不需要每次都算一遍。

这其实也是符合 DRY （Don't Repeat Yourself） 原则的，需要多次调用的数据，先一次性算好，要用的时候随时调取即可。