---
title: 前端提效套路1_CSS&JavaScript的位置
date: 2017-03-25 09:00
categories: fullstack
tags:
  - refactor
  - CSS
  - javascript
typora-copy-images-to: ipic
---

# CSS 上 JavaScript 下

最简单的提高效能方法，就是**调整CSS/Javascript的位置**。简单来说，就是

> 将 CSS 的代码放在最顶层读取
>
> 将 Javascript 代码放在最底层读取

![css_before](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-25-css_before.png)

![_js_after](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-25-_js_after.png)

那原理是什么呢？

当浏览器访问网站时，浏览器的运行原理是这样的：

1. 下载 HTML 本身
2. 读取 `head` 区块里的 CSS，允许页面逐步呈现
3. 浏览器根据 CSS 的定义来「绘制」网页

所以，CSS 应该放在最前面，也就是 `head` 区块里。

但是 JavaScript 却不一样。首先，JavaScript 因为是实现网页动态效果的，一般体积比较大，如果和 CSS 一起放在的话，CSS 的下载速度就会变得非常慢，从而拖慢了网页的下载速度。

另外，一般的 JavaScript 都有一个特点，那就是「等待 HTML 下载完成后再运行」，也就是我们常见的这一句代码：

```javascript
$( document).ready(function()) {
  console.log("ready!");
}
```

既然是网页的 HTML 下载完成之后才运行，那么把 JavaScript 放在最后再下载，不仅不会影响运行速度，反而会使得 HTML 的下载速度加快，使得用户在体验上觉得访问速度提升了。

# Rails 中的注意事项

新建一个 Rails 项目时，在 app/views/layouts/application.html.erb 里，JavaScript 和 CSS 默认都是放在 `head` 区块里的。

`application.html.erb` 是一个 layout 文件，默认下，所有 HTML 文件都会挂载这个 layout，而其他 HTML 挂载的位置就是 `application.html.erb` 中的 `<%= yield %>` 。

如果我们把 `application.html.erb` 里的 JavaScript 放到最后，也就是在 `<%= yield %>` 后面，而这时挂载的网页中又手写了一些 jQuery 代码的话，那么实际运行时，这些「 手写的 jQuery 代码」 是在 `<%= yield %>` 里的，而这时，jQuery 还没开（JavaScript 放在了最后），这些「手写代码」 就会烂掉。

![YDMR2coGQxGmh6MkHn6l_js_2](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-25-YDMR2coGQxGmh6MkHn6l_js_2.png)

解决办法是

1. 将手写的代码有 `content_for` 包起来
2. 在 `javascript_include_tag` 下面加一行 `<% yield :handwrite_javascript %>`

![ICPtH9rTQA2kzyBctMms_螢幕快照 2017-02-16 下午9.12.08](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-25-010600.png)

# 总结

提效简单办法

1. CSS 放在最前面的 `head` 区块里；
2. JavaScript 放在最后面的 `</body>` 前；
3. 在其他网页手写 JavaScript 代码时要特别处理一下。