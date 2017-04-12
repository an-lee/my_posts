---
title: ORID_恢复正常学习状态
date: 2017-02-02
categories: fullstack
tags:
  - ORID
---

## Objective

> 关于今天的课程, 你记得什么?
> 完成了什么?

今天按照购物网站的解答实作了一遍。包括网站的基础建设、购物网站的后台设计、管理员的权限设置、上传图片功能等。

其中上传图片用的 gem 是 carrierwave，在招聘网站实作时已经用过，当时是用来上传简历。这一次配合 carrierwave，还用到另一个 gem，叫 mini_magick。

mini_magick 的具体用法尚不清楚，但用 carrierwave 创建的 uploader 里，有一句代码

```ruby
include CarrierWave::MiniMagick
```

另外还有几句代码是设定上传图片的3种不同的显示尺寸。这个功能似乎是要 mini_magick 来实现的。

## Reflective

> 你要如何形容今天的情绪
> 今天的高峰是什么?
> 今天的低点是什么?

在按照教程的解答来实作，整个过程比较顺利，心情也是平静的。

## Interpretive

> 我们今天学到了什么?
> 今天一个重要的领悟是什么?

在实作的最后一步，叫做「重构后台商品列表」。「重构」的意思是重新构建，recontruction。在主要功能完成之后，对写出来而代码进行反思，然后通过「重构」使之更加「优雅」。

## Decisional

> 我们会如何用一句话形容今天的工作
> 有哪些工作需要明天继续努力?

今天练习时间大概2小时，恢复到正常的学习状态了，明天把 carrierwave 和 mini_magick 的相关资料学习一下。
