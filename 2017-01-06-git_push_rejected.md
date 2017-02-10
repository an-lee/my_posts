---
title: git_push_rejected
categories: fullstack
tags:
  - RoR
---

教程步骤：

> Rails 实战： 招聘网站
> [5-2 Pull Request 教学](https://fullstack.xinshengdaxue.com/posts/352)

按照教程，修改 README.MD 后做了一次`git push origin version-1` ，在后面的操作中因为出现了[诡异的bug](http://an-lee.pro/posts/2017/01/06/1278204)，把原来的 Project 删除重新来了几次。

做完了

> 套上 Bootstrap
> 安装 devise
> 安装 SimpleForm

后，再 `git push origin version-1` 时，出现了错误

![Screen Shot 2017-01-06 at 6.57.19 AM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1278238/LeRkk0CSXOIoIXcosBts_Screen%20Shot%202017-01-06%20at%206.57.19%20AM.png)

我大概是这么理解的，这个时候远程 Github 上的 version-1 和我本地的 version-1 存在不同时间了修改的不同代码，起了冲突。远程的 version-1 是修改了 README.MD，我在本地新建 Project 时就丢失了这次修改。

按照提示，我先 `git pull origin version-1` ，把远程的 Version-1 和本地的 Version-1 合并在一起，重新再 push……

还是不行，同样的错误。

对 Git 的操作还是不明就里，先放着吧，把远程的 Repo 也删了，重新来。
