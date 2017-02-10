---
title: 'rails_console出错问题的解决'
date: 2016-12-10 09:47
categories: [fullstack]
tags:
  - RoR
---
教程步骤：

> Rails 第二课：初级练习
> [3-8 把投票记录和 Topics 接起来](https://fullstack.xinshengdaxue.com/posts/47)

 在3-6跳过的问题(rails console 出错)，在3-8中再次出现，避无可避了。

 Google 了一下，找到一些解决办法，试了[这个帖子里的办法](http://stackoverflow.com/questions/16756287/cannot-execute-rails-console-due-to-an-error-with-readline)，无果。

 在 slack 上提问，不久收到助教 @qinfeng 的解答，办法是修改 Gemfile，如下图

 ![Snip20161210_2.png](http://user-image.logdown.io/user/22009/blog/21058/post/1190548/3HMeFzOdSfWO4CgEJpaa_Snip20161210_2.png)

 修改后，保存，再运行

```ruby
 bundle install
```

 再次运行

```ruby
 rails c
```

 得到预期结果

```
  anli@Ans-Mac-mini ⮀ ~/railsbridge/suggestotron ⮀ ⭠ master± ⮀ rails c
Running via Spring preloader in process 54532
Loading development environment (Rails 5.0.0.1)
 :001 >
```
