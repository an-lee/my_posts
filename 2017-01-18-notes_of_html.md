---
title: HTML自学笔记
categories: fullstack
tags:
  - RoR
  - HTML
  - cheatsheet
---

## div / span 的不同

要理解 div 和 span 的不同，首先要理解两个概念：块级元素（block-level element） 和内联元素（inline element）。

块级元素总是新起一行，而且会占满屏幕的宽度。内联元素不会新起一行，仅仅根据内容的多少决定其宽度。常见的块级元素有：
`<di>`,` <h1>`~`<h6>`, `<p>`, `<form>`等。常见的内联元素有：`<span>`, `<a>`, `<img>`。

再简单点来说，在块级元素里的内容总是独立成段，而内联元素里的内容则仍然是在原来的句子或段落当中。

## class 与 id 的不同

class 和 id 都是 CSS 的一个选择器（selector），在 CSS 从定义好特定格式，在 HTML 中可以直接引用。但两者又有不同。

class 和 id 的最大不同点就是，class 是可以重复利用的，id 是唯一的。怎么理解呢？

class 定义之后，可以被多个 HTML 元素（element）调用，一个元素也可以同时调用多个 class。

比如说定义两个 class，分别是

```css
.center {
    text-align: center;
}
.red {
    color: red;
}
```

可以这么调用

```html
<div class="center" >Hello World!</div>
<div class="center red">Hello World!</div>
```

而 id 呢，一个 id 在一个页面内只能调用一次

```css
#para1 {
    text-align: center;
    color: red;
}
```

调用时

```html
<p id="para1">Hello World!</p>
```

简单来说，当一种格式需要多处调用时，用 class；如果是需要单独识别的，就用 id。使用 id 的一个常见例子就是，用来作为页面内的定位。比如说定义了一个 id 名为 `comments`，在页面某处定义之后，使用链接 `http://yourdomain.com#comments` 就可以让浏览器将网页滚滑到 comments 的地方。

这两者的关系确实比较难理解，我 google 了一下，找到了一篇解释得非常到位的[文章](https://css-tricks.com/the-difference-between-id-and-class/)，里面用了一个非常恰当的类比来说明 class 和 id 的区别。这个类比就是条形码和序列号。

拿 iPhone 举例，每一部 iPhone 的包装盒上都会用条形码和序列号，它们的作用是不一样的。每一款相同型号的 iPhone，比如说128 g 的黑色 iPhone 7，它们的条形码都是一样的，这个条形码上记录的信息是这个型号 iPhone的价格和库存等。而每一部 iPhone 的序列号是不一样的，序列号是每一部 iPhone 的「身份证」，记录的是每一部手机的出厂日期、激活日期、保修时长等唯一的信息。在 CSS 里，class 就好比是条形码，而 id 就好比是序列号。



## p 与 br 的不同

p 的意思是 paragragh，也就是段落的意思。在 HTML 里，用标签 `<p>`…`</p>` 来表示一个段落的文字。在这对标签内的所有空格、换行都会被 HTML 忽略掉，显示出来的效果就是一个且只有一个连续的段落。如果要得到多个段落的显示效果，那就得用多个 `<p>`…`</p>` 了。

例如

```html
<p>
  line1.
  line2.
</p>
```

虽然代码中换了行，但是 HTML 是不认的，显示的效果是

> line1.line2.

要实现换行，可以用两个 `<p>` 标签

```html
<p>
  line1.
</p>
<p>
  line2.
</p>
```

这样的显示效果就是

> line1.
>
> line2.

br 的意思是 line break，也就是换行的意思。在 HTML 文档里，直接打回车键换行，是不会被识别的，也就是需要换行的时候，必须用 `<br>` 来实现。另外，`<br>` 可以嵌入到 `<p>`…`</p>` 内使用。

例如

```html
<p>line1. <br> line2. </p>
```

显示的效果是

> line1.
>
> line2.

## 如何使用 table 排版

HTML 中的排版，一般有4种，分别是

- HTML tables
- CSS float property
- CSS framework
- CSS flexbox

而 [w3schools](http://www.w3schools.com/html/html_layout.asp) 明确表示，不要用 table 来排版。 table 只是用来作为显示表格式数据的，用来排版，虽然也可以做出好看的排版格式，但是会使得代码变得过于复杂，以至变得一团糟，非常不利于后期修改和维护。
