---
title: ORID_要进步|做项目
date: 2017-04-12 04:45
categories: fullstack
tags:
  - ORID
  - Rails
---

昨天用在 Rails 上的时间比较少，但也学到了一些新知识。

在想一个个人项目的时候，想要在数据库的一个栏位记录数组，甚至哈希值。经过 Google，发现这是可以做到的。具体做法是，该栏位类型用 `text`，然后再通过 `serialize` 这个方法就能[把数据以数组或者哈希保存](http://api.rubyonrails.org/classes/ActiveRecord/Base.html#class-ActiveRecord::Base-label-Saving+arrays-2C+hashes-2C+and+other+non-mappable+objects+in+text+columns)。其实是以 YAML 的格式保存。

昨天晚上翻 Ruby 的语法书，很多之前的疑惑逐渐清晰起来。主要的概念

- 在 Ruby 中，表现数据的基本单位称为对象（object）


- 对象的种类就是所谓的类（class）
- 实例（instance）实际上就是属于某个类的对象，实例和对象的意义几乎等同
- 对象的所有操作被封装成方法（method）
  - 实例方法
  - 类方法
  - 函数方法

有时候觉得自己对 Rails 已经有一点把握了，有时候又会觉得自己还差得远，心里便产生焦虑。要追求再一次的突飞猛进，唯一方法就是继续做真实项目，做「完整的产品」。