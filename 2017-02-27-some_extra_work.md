---
title: ORID_有些弯路必须走
date: 2017-02-27 05:36
categories: fullstack
tags:
  - ORID
typora-copy-images-to: ipic
---

前两天是周末，我哪也没去，一直在家里对网站 TimeEx 进行改版。周末的时候还是把队友喊到我家来一起干活。

周六一天，我把商品页面进行了改版，队友也把评分功能写好了。由于我是那么地迫切将网站改版，好去发帖拉票，毕竟比赛还剩几天了。每当写完一个新功能，就 pull request ，只要没有 conflict，我就马上 merge。有时候，我竟然忘记建立新的分支，直接在主分支 master 上就改起来。

教程上更新了七牛云的教程，虽然我的 AWS 早已经设定好了。我还是忍不住要试一下。结果就试出事了。

改成七牛云之后，在本地测试正常，但部署到 heroku 就会报错。我反复测试了好多遍，还是无果。为了不影响网站正常运行，就先改回 AWS。结果发现 AWS 也失效了。我开始犯迷糊，由于没开新分支，这一系列的改动污染了主分支 master。我花了很大的时间去找 bug，无果。

但是 Github 是时光机啊，应该可以回到过去。Google 之后，找到办法

```
git reset --hard <old-commit-id>
git push -f <remote-name> <branch-name>
```

![Screen Shot 2017-02-27 at 5.59.19 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-02-26-220035.png)

我首先在本地切换到之前正常的分支，尝试 deploy，确定没问题之后，到 Github 上找到该次 commit 的 SHA。经过测试，我确定 sendcloud settings 这次 commit 最新的正常 commit，其完整 SHA 是 `9b97bbb705ddc42d3d7d3349a0a31883f30a6d42` 。切到被污染的 master 分支，在 iTerm 输入

```
git reset --hard 9b97bbb705ddc42d3d7d3349a0a31883f30a6d42
```

就会发现本地的 master 已经撤回到 `sendcloud settings` 这次 commit 了，其后的 commits 已经全部删除。再把本地的 master 强制覆盖 Github 上的主分支

```
git push -f origin
```

就会发现 Github 也神奇地 **回到过去** 了。

需要注意的是，这个办法是无法撤回的，在 StackExchange 上，回答者有明显的提示

> Note: As written in comments below, **Using this is dangerous in a collaborative environment: you're rewriting history**

这一次的事故其实完全可以避免，只要严格遵守时光机使用方法

1. 建立分支
2. 增加feature
3. 本地测试
4. 将该分支 push 到 Github
5. 将该分支部署
6. 确认部署没问题之后，再 merge 到主分支

我突然意识到，要一个网站长期稳定地运行，是一件多么困难的事情。