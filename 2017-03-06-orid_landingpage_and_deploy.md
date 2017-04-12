---
title: ORID_LandingPage和部署
date: 2017-03-06
categories: fullstack
tags:
  - ORID
  - Rails
typora-copy-images-to: ipic
---

过去的一周，因为工作上和生活上都特别忙，全栈营编程比赛的作品变得无暇顾及，几乎没有更新迭代。在周末的时候，终于抽出时间做了两件事。

# Landing Page

Landing Page 的套路比较固定，

1. 一句话形容自己的好处
2. 使用此服务的三大好处
3. 叙述原理
4. 使用者见证
5. Call to Action
6. 消除疑虑

老师一再强调，顺序一定要按照这个来，不可以调换。其实这是一步一步诱导消费者「入坑」的套路。一环扣一环，顺序乱了，整个套路就不成立了。

教程中制作 Landing Page 可以采用网上的服务 http://unbouncepages.com/ 在线制作。根据提供的模板，直接排版设计，所见即所得，非常方便。

如何把用 unbouncepages 制作好的 Landing Page 嫁接到自己的网站上呢？论坛上有热心的同学给出了方法，非常简单粗暴，就是在浏览器上查看 Landing Page 的代码，直接复制粘贴到自己的网站上即可。

这个方法，我试过了，并不是特别好用。可能因为我选的模板本身的结构相对复杂，最后的代码有好几千行，其中大部分是 CSS 和 JS 代码。也由于我的网站本身的 CSS 设定较多，复制过去，很多效果就乱套了。

最后，我把 unbouncepages 制作的 Landing Page 当作「效果图」，在我的网站上用代码「临摹」下来的。

![screencapture-time-ex-herokuapp-1488705748483](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-05-screencapture-time-ex-herokuapp-1488705748483.png)

# 部署到linode

全栈营的部署教程并没有写得十分详细，经过多次尝试，也 Google 了很多辅助材料，在周六的时候，除了把教程走通，也成功把 Rails101 部署了。接下来，我想把我的 Time-Ex 部署。

与 Rails101 不同，JDStore 部分，为了上传图片，申请了 AWS 的服务，又为了避免 AWS 密码泄露，我们安装了 figaro 来管理密码。

经过 Google，发现其实 figaro 是专门针对 Heroku 的一个插件，可以把保存在本地的密码通过 figaro 直接在 Heroku 的环境里设置好，如此一来，我们的密码就不需要保存在 Repo 里，避免了风险。

而当我要部署到 linode 时，因为 linode 本来就是我购买的虚拟主机，是可以直接保存图片的，因此其实并不需要在使用 AWS 来保存图片（Heroku 不支持直接上传图片）。

因此，要把 JDStore 部署到 linode，可以把用来上传图片到 AWS 的工具 fog 和管理密码的 figaro 这两个 gem 卸载，carrierwave 上传图片的方法，直接改成跟本地一样，也就是 `storage: file` ，就可以了。

成功部署： www.1timex.com

虽然已经成功部署，但是对于服务器上的数据库该如何管理还是一头雾水，要继续多试几次。