---
title: ORID_用测试来写需求
date: 2017-04-06 08:00
categories: fullstack
tags:
  - Rails
  - ORID
---

昨天学习了 xdite 老师的 [RSpec101](https://xdite.gitbooks.io/rspec-101)，实作了几个 controller 的 RSpec，体会到了 TDD 方法的先进理念。同时在网上读了不少关于测试的文章，其中印象最深的一句话是

> 写测试，其实就是把需求用测试写出来。

有种脑袋被击中的感觉。

一般的开发顺序是

> User Story => Coding => Test => Refactoring => Test => ...

如果由 Test 来驱动的话，其实就是用 Test 文档来表述了 User Story。从这个角度来看，那么一份好的 Test 最重要的就是能**完整且准确**地表述 User Story。如果说 Test 是用来表述 User Story，那就得逻辑清晰，简单明了。

能写一份好的 Test 文档，也是协作能力的重要体现，是与其他同伴的协作，也是与不同时空的自己协作。

加油吧，多写几份 RSpec 练习一下。