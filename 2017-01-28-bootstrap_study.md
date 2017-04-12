---
title: Bootstrap学习
date: 2017-01-28
categories: fullstack
tags:
  - Rails
  - bootstrap
---

# 在魔改大赛的同时把 Bootstrap 学了

Job-listing 魔改大赛的一个重点是学习在 Bootstrap 的框架下美化网站。

根据助教提供的教程，熟悉了基本操作之后，有了一点美化的思路，但这远远不够。

我开始的想法是，挑一个漂亮的 Bootstrap 网站，把它 copy 过来，然后此基础上修改。但是由于对 Bootstrap 太不熟悉了，自己瞎弄了好多天，总是有很多的 bug。

直到我发现在 [w3schools](http://www.w3schools.com/bootstrap/bootstrap_templates.asp) 上，除了有 Bootstrap 的系统教程，更难得的是，上面有3个**完整的 Bootstrap Themes 例子**，教我们一步一步地**从零开始写**一个漂亮的 Bootstrap 首页。

比如说最简单的例子 "[Simply Me](http://www.w3schools.com/bootstrap/bootstrap_theme_me.asp)"，实现步骤分为

1. HTML Start Page
2. Add Bootstrap CDN and Put Elements in Container
3. Add Background Color and Center Text
4. Shape the image to a circle with the `.img-circle` class
5. More Content
6. Add Padding
7. Add a Button
8. Add an Icon
9. Modify The Tird Container (Add Grid)
10. Make The Images Responsive
11. Add a Navbar
12. Style The Navbar
13. Add a Footer
14. Final Touch
15. Comlete "Simply Me" Theme

经过这样详细的拆解，跟着教程一步步把例子做下来，Bootstrap 的各个基本功能的实现就一清二楚了。

我魔改大赛的作品就是在 w3school 上的例子：[Bootstrap Theme "The Band"](http://www.w3schools.com/bootstrap/bootstrap_theme_band.asp) 的基础上修改完成的。

其中部分的功能有以下这些

# Carousel 实现图片轮播

![job-1](http://okgqgpbx3.bkt.clouddn.com/blog/2017-01-28-001203.jpg)

这里有一个坑，就是如果你用来轮播的图片比例不一样，在图片切换的时候，网页的整个比例都会变化，看起来很别扭。所以最好先把所有图片裁剪成统一的比例。

# 将图片统一风格

我的网站风格是黑白色调的，在找图片的时候，有些漂亮的图片想用，但却是彩色的。当然可以用 photoshop 把图片处理成黑白的，但其实可以用简单的 css 代码就可以把图片色彩统一成黑白

```css
.carousel-inner img {
    -webkit-filter: grayscale(90%);
    filter: grayscale(90%); /* make all photos black and white */
    width: 100%; /* Set width to 100% */
    margin: auto;
}
```

比如说，我找到的原图是这样的

![architect-wallpaper-008](http://okgqgpbx3.bkt.clouddn.com/blog/2017-01-28-1202.jpg)

经过 CSS 的美化，最终的显示效果是这样的

![Screen Shot 2017-01-28 at 7.29.42 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-01-28-001200.jpg)

# Collapsibles 点击展开

实现的效果这样的

![job-2](http://okgqgpbx3.bkt.clouddn.com/blog/2017-01-28-001147.jpg)



# tab 分栏

把工作按照分类显示

![job-3](http://okgqgpbx3.bkt.clouddn.com/blog/2017-01-28-001201.jpg)



# 更多

想了解得更详细，可以到 w3schools 的 [Bootstrap Theme "The Band"](http://www.w3schools.com/bootstrap/bootstrap_theme_band.asp) 上学习。

我的魔改大赛作品首页是 [EA OFFER](https://eaoffer.herokuapp.com/) ，Github 地址是 [an-lee](https://github.com/an-lee/job-listing-base/)。

我的魔改大赛作品上线之后被收录到了[推荐作品](https://fullstack.xinshengdaxue.com/competitions/1/list_all?selection=recommendation)栏目。魔改大赛投票截至日期马上就到了，如果你还没有把手上的5票投完，请为我投一票吧，投票地址在这里 [https://fullstack.xinshengdaxue.com/works/95](https://fullstack.xinshengdaxue.com/works/95) 。
