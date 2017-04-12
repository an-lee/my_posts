---
title: 几个诡异的bug
date: 2017-01-06
categories: fullstack
tags:
  - Rails
---

这一周正式开课，进入Rails实战，但我的实作却一直卡在了最开始几步。按道理来说，Rails 101我刷了5遍，理应更顺利才对，但是事实再一次证明，哪里来的那么多「理应」？

# 诡异 bug 1: undefined method

昨天早上，在用 flash helper 做提示讯息时，整个操作完成后，讯息没出现，倒是出现了错误

```
undefined local varible or method 'user_facing_flashes' for #<#<class:0x007fe3ee340f88>:0x007fe3f24df570>
```

![Screen Shot 2017-01-05 at 6.27.06 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1278204/9hprLFUfR8ZfVcuvovew_Screen%20Shot%202017-01-05%20at%206.27.06%20AM.png)

怎么可能呢？我反复对照了所有代码，甚至全部按照教程的代码复制粘贴了一遍，还是这个错误。

我把 Project 删除，再来一遍，同样的错误。再删除，再复制粘贴一遍，同样的错误……

重复了4遍，仍旧是一样的错误，昨天的上午就这样崩溃了。

这几步在 Rails101 里做了5遍了啊，也没出现这样的错误。

昨天晚上下班回家，深呼吸，重新新建 Project，再来一遍，居然好了！

太诡异了……

# 诡异 bug 2:  bootstrap-sprockets

今天早上由于忘记 `git commit`，就新建了一个新 `branch`，导致**打回原形**，只好重来。

这一次，才刚做到挂 bootstrap 这一步就出错了……

```
File to import not found or unreadable: bootstrap-sprockets.
```

![Screen Shot 2017-01-05 at 9.24.18 PM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1278204/LzkpmkFdSMSifb9pLkHJ_Screen%20Shot%202017-01-05%20at%209.24.18%20PM.png)

我才操作了几步而已啊！反复对比了代码，又复制粘贴了一遍，确认没错。

将错误信息 Google 了一下，答案都是说 `Gemfile` 写错了，`application.scss` 写错了，或者没有 `bundle insall`，没有 重开 `rails s` ……

`Gemfile` 没写错，`application.html.erb` 也没写错，`application.scss` 没写错，也`bundle install` 了，重开了 `rails s` ，还是不行。

找不到原因，只好 `rm -rf` 删除，重新再来。

这一次，想起一件事，就是 macOS 10.2 的一些bug，就先把 `Gemfile` 里的 `spring` 给 `#` 掉了。

顺利把这一步做过去了。

---
**update:**

在Slack上发现有同学遇到相同的问题，助教给出了正确的解法。

> 主要原因是 bootstrap 没有安装成功
>
> bundle install 重新安装一遍

怎样重新安装呢？助教提供的方法是将 ` app/assets/javascript/application.js` 的两句注释删除，然后重新 `bundle install`。

![pasted_image_at_2017_01_05_10_24_pm.png](http://user-image.logdown.io/user/22009/blog/21058/post/1278204/28EfiQsfT5S6dGS1pFHL_pasted_image_at_2017_01_05_10_24_pm.png)
