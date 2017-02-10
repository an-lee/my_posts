---
title: '写ORID的模板:Atom的Snippets'
categories: [fullstack]
tags:
  - snippets
---

用 Atom 的 Snippets 可以大量减轻程序员重复敲打键盘的工作量，另一方面，也可以当作写作模板来用。

全栈营的学习要求每天用 ORID 的思考框架来写总结，既然是思考框架，写作是就可以先把整个框架复制过来，再一步步填写即可。

于是就想到用 Atom 定制一个 ORID 的 Snippets，尝试了几次，Google 了一下，最后成功了。

在 Atom 里，`cmd`+`shift`+`P`打开 Command Palette，输入`Open Your Snippets`打开文件`snippets.cson`。在文末增加以下代码

```
'.source.gfm':
  'ORID':
    'prefix': 'OR'
    'body': """
      ---
      title:${1:title}
      ---

      # Objective
      > 关于今天的课程, 你记得什么?
      > 完成了什么?

      ${2:}

      # Reflective
      > 你要如何形容今天的情绪
      > 今天的高峰是什么?
      > 今天的低点是什么?



      # Interpretive
      > 我们今天学到了什么?
      > 今天一个重要的领悟是什么?



      # Decisional
      > 我们会如何用一句话形容今天的工作
      > 有哪些工作需要明天继续努力?


    """
```

保存后，新建一个`.md`文件，输入`OR`后，再按`tab`，整个 ORID 的框架就生成了。效果如下图：



在 Atom 写完之后，复制到 logdown 发表。
