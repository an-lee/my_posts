---
title: Rail101第五遍还在犯的错误
date: 2017-01-02
categories: fullstack
tags:
  - Rails
---

今天从早上6点到中午11点，花了总计5个小时，把 Rails101 做了第五遍。

虽然已经是第5遍，一些低级错误仍然在犯，熟练程度仍旧远远不够。在第5遍里，卡壳时间最长的两个错误都是语法错误。

## 错误1：`<% %>`和`<%= %>`

在做6-3时，完成 step 4 之后发现页面没有显示 Post，如下图所示

![WX20170102-094309.png](http://user-image.logdown.io/user/22009/blog/21058/post/1266741/Nkmz8A7sQcSH4Hqrueoe_WX20170102-094309.png)

页面没有显示错误，但是显示不正常。检查代码，来来回回看了好多遍，终于发现错误：

> 该用`<%= %>`的地方用了`<% %>`

![WX20170102-094338.png](http://user-image.logdown.io/user/22009/blog/21058/post/1266741/tlrubetPTRibXGAEucHR_WX20170102-094338.png)

![WX20170102-094415.png](http://user-image.logdown.io/user/22009/blog/21058/post/1266741/ukzPnct4TiqbltBVNCV9_WX20170102-094415.png)

对于什么情况用什么，到现在也不是特别清楚，但经常出现的错误是漏打了`=`号。这个错误从第一遍开始就经常犯。一定要长点心！

## 错误2：误删了一个`)`

在做8-3时，完成 step 5 之后，页面出现错误，如下图所示

![WX20170102-105827.png](http://user-image.logdown.io/user/22009/blog/21058/post/1266741/bTq8csd4SVeRGGcjNAta_WX20170102-105827.png)

提示的错误是在 posts_controller.rb，检查很多遍之后，这个文件里的代码很少，没检查出错误。我猜想还是 index.html.erb 出了问题。逐行检查代码，来回很多遍也没看出错误。

 我干脆把教程上的代码复制过来，保存，刷新，页面马上正常了。很明显，问题就出现在这几段代码的某个地方。

 我再次逐行检查，还是无果。我把教程上的代码逐段替换来检查，终于把错误代码锁定在一个小范围内。又来回检查好多遍，终于找到了原因，竟然是漏了一个`)`！

![WX20170102-113242.png](http://user-image.logdown.io/user/22009/blog/21058/post/1266741/MrPmxcNRMudABM93LZSw_WX20170102-113242.png)

![WX20170102-113347.png](http://user-image.logdown.io/user/22009/blog/21058/post/1266741/9wrDHC35TlmIsGqqKdV4_WX20170102-113347.png)


 在 Atom 里打括号都是自动保存的，这个地方应该是我不小心删除了。一个小错误，浪费那么多时间！一定要长点心！
