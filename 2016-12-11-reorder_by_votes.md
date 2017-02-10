---
title: '加分题：根据投票分数排序'
categories: [fullstack]
tags:
  - RoR
---

教程步骤：
> Rails 第二课：初级练习
> [3-13 加分题 & 下一步](https://fullstack.xinshengdaxue.com/posts/39)

第二道加分题：根据投票分数排序。原教材中并没有找到关于排序相关的内容，在中有一个外链接 [Active Record Query Interface RailsGuide](http://guides.rubyonrails.org/active_record_querying.html)，在里面找到一个看似相关的 method `reorder`。翻看`/app/controllers/topics_controller.rb`、`/models/topic.rb`和`/view/index.html.erb`等文件，找不到该怎么用`reorder`这个 method ，乱试了几遍，还是失败了。

来回捅了一个小时，放弃。上 Google 搜索`"intro to rails" + "Sort topics"`，居然找到一位自愿助教，在[这里](https://github.com/rscottbradshaw/Suggestotron/commit/534436025a55885c30ddb0a2d10c3031c2cf81cc)找到了答案。

方法是，在`/app/controllers/topics_controller.rb`中的`index`里增加一句排序的语句。

```ruby
@topics = @topics.sort_by{ |topic| topic.votes.count }.reverse
```

修改完是这样的：

```ruby /app/contollers/topics_controller.rb
def index
  @topics = Topic.all
  @topics = @topics.sort_by{ |topic| topic.votes.count }.reverse
end
```

用到的语句是`sort_by`而不是`reorder`。

不知道`reorder`是怎么用的。后续学习要留意。

这个加分题不是完全「靠自己」解出来的，是 google 出来的。但生活本身就是开卷考试，重点在于**解决问题**。
