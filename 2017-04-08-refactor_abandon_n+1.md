---
title: 后端提效套路1_消灭N+1
date: 2017-04-08 10:30
categories: fullstack
tags:
  - Rails
  - refactor
typora-copy-images-to: ipic
---

在 Rails 里提取数据库资料时，最常遇到的一个问题就是 **N+1 Query**。下面用一个具体问题来说明。

# 什么是 N+1 Query

比如说有两个 model，一个是学生（student），另一个是班级（class）。这两个 model 的连结应该是**一对多**的关系，也就是说

- 一个 class 里可以有很多 students
- 一个 student 只可以属于一个 class

在 model 里

```ruby
# class Model
has_many :students

# student Model
belongs_to :class
```

在网页中，我们要列出所有的学生，已经各个学生的班级名字。

在 controller 里，我们一般这么写

```ruby
def index
  @students = Student.all
end
```

在 view 里，我们一般这么写

```ruby
<% @students.each do |student| %>
<tr>
  <td> <%= student.name %> </td>
  <td> <%= student.class.name %> </td>
</tr>
<% end %>
```

这就是我们最常见的写法，看起来没什么问题，但是，如果去看实际调用数据库的 log，就会发现调用的次数太多了。假如调取 4 条 student 的资料，因为要显示其对应的班级名称，实际调用数据库的次数会是 4+1 次。

| 姓名   | 班级    |
| ---- | ----- |
| 王大明  | 一年级1班 |
| 李小全  | 二年级2班 |
| 高二飞  | 三年级4班 |
| 李四宏  | 四年级1班 |

实际调用的 log 可能会是

```sql
STUDENT Load (0.1ms) SELECT * FROM "students"
CLASS Load (0.1ms) SELECT * FROM "class" WHERE "student_id" = 4
CLASS Load (0.1ms) SELECT * FROM "class" WHERE "student_id" = 5
CLASS Load (0.1ms) SELECT * FROM "class" WHERE "student_id" = 6
CLASS Load (0.1ms) SELECT * FROM "class" WHERE "student_id" = 7
```

# N+1 Query 有什么后果

如果是在本地的开发环境，看着每一条查询时间都是零点几毫秒，似乎没什么关系。

- 每调取一笔资料耗时 0.1 ms
- 总共调取 1+ N（1 + 4 = 5）笔资料
- 总共是 0.1 *5 = 0.5 ms

但是如果是正式开发环境，要是 Web server 和 Database Server 在不同的机器，调取每一笔资料可能就是会是 20 ms 的差距，当一个页面有 20 笔资料的时候，效能数据就会变成这样

- 每调取一笔资料耗时 20 ms
- 总共调取 1 + N （1 + 20 = 21）笔资料
- 总共是 21 * 20 = 420 ms

这就会变成一个灾难，这个网页会变得巨慢无比。

# Rails 里应该怎么做

那如何消灭 N+1 Query 呢？在 Rails 其实很简单，在 controller 里把调取的代码改成这样就好了。

```ruby
def index
  @students = Student.includes(:class).all
end
```

因为加了一个 `includes` ，Rails 就会智能地把多个 query 合并成一个 query 了。

```sql
STUDENT Load (0.1ms) SELECT * FROM "students"
CLASS Load (0.1ms) SELECT * FROM "class" WHERE "student_id" IN (4, 5, 6, 7)
```

这样，实际上就是通过[减少查询次数](http://an-lee.pro/2017/04/08/2017-04-08-refactor_activerecord/)来达到了提升数据库效率的目的。