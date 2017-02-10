---
title: 自学CSS笔记
categories: fullstack
tags:
  - RoR
  - CSS
  - cheatsheet
---

自学 CSS 笔记

## 什么是 CSS

CSS 的全称是 Cascading Style Sheets，中文直接翻译是「层叠样式表」。一个网页，HTML 负责内容和结构，CSS 则负责其表现样式，也就是说 CSS 是用来装饰网页的。

## CSS 的语法

CSS 的定义方式是 `选择器(selector)` + `声明(declaration)` 。

比如说

```css
p {color: red; text-alingn: center;}
```

上面的这个例子里，`p` 就是选择器。选择器可以是 HTML 的自带元素名，比如说 `h1` , `h2` , `p` , `body` 等等，也可以是 `id` ,  `class` , `attribute` 等等。

`{}`内的就是声明。声明是由属性(property)和值(value)，可以同时定义多个属性，以 `;` 间隔即可。上面的例子里就定义了两个属性，分别是`color` 和 `text-alingn`，两者的值分别是 `red` 和 `center`。

整个 CSS 语句的意思是，在 `p` 的段落里，字体颜色是红色(red)，文字居中排列(center)。

## 如何引用 CSS

引用 CSS 的方法有3种，分别是

- 外部样式文件
- 内部样式文件
- 行中定义

常用的方式是第一种，把 CSS 定义统一在外部文件里，然后在网页的 HTML 里直接引用即可。在 ruby on rails 里常用的一个 gem，即 boostrap，其实就是把 bootstrap 这个样式引用进来，写 `html.erb` 文件时就可以直接用了。在 ruby on rails 的 web app 里，目录 `app/assets/stylesheets/` 下的 `.scss` 也是外部样式文件，在这些文件中定义好，即可在网页中引用。

内部样式文件指的是在某个具体网页里，在正文前面的 `<head> </head>` 里，用 `<style>…</style>` 来定义的 CSS 样式。这样定义的样式，只能在该网页内引用。比如说下面这个例子

```html
<head>
<style>
body {
    background-color: linen;
}

h1 {
    color: maroon;
    margin-left: 40px;
}
</style>
</head>
```

行中定义，就是在具体的网页中任意一个 HTML 元素内定义样式。比如说

```html
<h1 style="color:blue;margin-left:30px;">This is a heading.</h1>
```

这样的定义，就只对这一个元素内起作用。

如此对比下来，很容易可以得出一个结论，那就是尽可能用外部样式文件来写 CSS，因为这样修改起来最容易。只要 HTML 中的结构足够清晰，那么只要修改外部的 CSS 文件，就可以将同一个网站改头换面。

## CSS 的四向定义

在 CSS 中有很多属性通常有4个方向的，比如说文本边界线的样式( `border-style`)、线宽(`border-width`)、线颜色(`border-color`)，定义的时候，后面带的值数量不一样，意思也不一样。

如果定义4个值，那这4个值依次定义的是**上边**、**右边**、**下边**、**左边**。比如说

```css
{border-style: dotted solid double dashed;}
```

这个定义的意思是，上边界线样式是 `dotted`，右边界线样式是 `solid`，下边界线样式是 `double`，左边界线样式是 `dashed`。

如果定义3个值，那这三个值依次定义的是**上边**、**左边和右边**、**下边**。也就是说左边和右边的值相同，有中间的值定义。

如果定义2个值，那这两个值依次定义的是**上边和下边**、**左边和右边**。也就是说上边和下边的值相同，左边和右边的值也相同。

如果只定义1值，那么就是四边的值都相同。

同理，在 `marging` 和 `padding` 属性定义时，也一样。比如说

```css
h1 {margin : 10px 0px 15px 5px;}
```

这个定义的意思是，

> margin-top = 10px
>
> margin-right = 0px
>
> margin-bottom = 15px
>
> margin-left = 5px

## margin 与 padding 的差异

magin 定义的是一个元素边界线外面的空白区域大小，也就是说，margin 定义的是两个元素的边界线之间的距离。

而 padding 第一的是一个元素边界线内部的空白区域大小，也就是说，padding 定义的是元素内部文字与其边界线的之间的距离。

如果有两个元素，没有背景色，边界线也不显示，分别用 margin 和 padding 定义相同的值，其显示效果应该是一样的。如果有了背景色，或者元素的边界线显示出来，那么用 margin 定义效果是，元素的大小不变，两个元素之间出现了空白区域。如果用 padding 定义，其效果则是，元素的大小变大了，两个元素的边界是连着的。

## 什么是 box model

每个 HTML 元素其实可以看做是一个盒子（box），而 CSS 的「盒子模型」( box model )就是用来表述包裹 HTML 元素的层级。从外到内，有4个层级，分别是**Margin**, **Border**, **Padding**, **Content**，如下图所示。

![BOX MODEL](https://ww2.sinaimg.cn/large/006tKfTcgy1fby16mc9j5j30hm0b2dg2.jpg)

Content 就是 HTML 元素的内容，通常是文字或者图片。

Padding 的中文意思是防护垫，是指内容和边界线之间的区域，是透明的。

Border 就是边界线，围绕者 content 和 padding，可以定义形式、颜色、大小等等。

Marging 的中文意思是页边空白，是指边界线以外的空白区域，也是透明的。

有一点**需要注意**：

> 用 height 和 width 来定义一个元素(element)大时，其实定义的是 content 的大小，要计算这个元素(element)实际占用区域大小时，需要加上 Padding 的宽度、边界线(Border)的厚度、页边距(Margin)的宽度。

## 为何要使用 em 而非 px 来定义字的大小

字体的大小有两种定义方式，一种是绝对大小，一种是相对大小。

如果用绝对大小来定义，即用 px 来定义字体大小，用户在浏览网页的时候，不论用什么设备来浏览，网页的字体大小都是固定的。显然这样会让网站的兼容性变得很低。除非这个网站是用特定设备来浏览的。

相反，用相对大小来定义，即用 em 来定义，用户在不同的浏览器上浏览是，字体可以相应协调，网站在不同终端上都可以美观地显示出来。

比如说，段落的字体大小默认是 16 px，用相对大小来定义字体时，1 em 就是 16 px。
