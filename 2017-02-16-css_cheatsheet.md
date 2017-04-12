---
title: CSS_cheatsheet
date: 2017-02-16
categories: fullstack
tags: 
- CSS
- cheatsheet
---

在学习别的主题模板时，CSS 往往写得无比复杂，看得我头昏眼花，其中一个重要原因就是，我对使用 CSS 选择器规则一头雾水。因此，将 CSS 常用的选择器用法总结如下。

# *<u>.class</u>*

## CSS 范例

```css
# CSS Version: 1
.intro {
  background-color: yellow;
}
```

## HTML 范例

```html
<div class="intro">
  <p>
    This area will be styled.
  </p>
</div>
```

## 解释

*<u>.class</u>* 选择器可以定义任何一个类别，在 HTML 中用 `class= class_name` 来调用。

---

# *<u>#id</u>*

## CSS 范例

```css
# CSS Version: 1
#firstname {
  background-color: green;
}
```

## HTML 范例

```html
<p id="firstname">This area will be style.</p>
```

## 解释

*<u>#id</u>* 选择器可以定义 `id=#id_name` 的样式，需要注意的是一个 id 在一个页面内只能调用一次。

---

# <u>*</u>

## CSS 范例

```CSS
# CSS Version: 2
* {
  font-family: "Times New Roman", Times, serif; 
}
```

## HTML 范例

```html
All context will be styled.
<p>
  All context will be styled.
</p>
<div>
  All context will be styled.
</div>
```

## 解释

<u>*</u> 选择器将定义所有的元素的样式。

---

# *<u>element</u>*

## CSS 范例

```css
# CSS Version: 1
p {
  background-color: yellow;
}
```

## HTML 范例

```html
<p>The area will be styled.</p>
```

## 解释

*<u>element</u>* 选择器将定义任意一个元素的样式，*<u>element</u>* 可以是任意一个 HTML 元素。

---

# *<u>element, element</u>*

## CSS 范例

```CSS
# CSS Version: 1
h1, p{
  background-color: yellow;
}
```

## HTML 范例

```html
<h1>
  The area will be styled.
</h1>
<p>
  The area will be styled.
</p>
```

## 解释

这里的逗号是并列的关系，将相同样式的元素统一定义，与分开定义效果一样。

---

# *<u>elment1 element2</u>*

## CSS 范例

```css
# CSS Version: 1
div p{
  background-color: yellow;
}
```

## HTML 范例

```html
<div>
  This area will not be styled.
  <p>
    This area will be styled.
  </p>
</div>
```

## 解释

*<u>elment1 element2</u>* ，两个 *<u>element</u>* 用空格并列，定义的是所有在 *<u>element1</u>* 里的 *<u>element2</u>* 的样式。

---

# *<u>element1>element2</u>*

## CSS 范例

```css
# CSS Version: 2
div > p {
  background-color: yellow;
}
```

## HTML 范例

```html
<div>
  This area will not be styled.
  <p>
    This area will be styled.
  </p>
  <span>
  	<p>
      This area will not be styled.
    </p>
  </span>
</div>
```

## 解释

两个 *<u>element</u>* 用 `>` 来连接，表示两者的 **父子元素** 的关系，后者是前者的子元素，前者是后者的父元素。特别注意的是，用 `>` 符号表示，后者必须是前者的 **直接子元素** 才会被选择。如上面的例子，当 `<p></p>` 在 `<span></span>` 里，而 `<span></span>` 又在 `<div></div>` 里时， `<div></div>` 和 `<span></span>` 都是 `<p></p>` 的父元素，但 `<p></p>` 的直接父元素是 `<span></span>` ，所以在这个例子里不会被选择。

---

# *<u>element1+element2</u>*

## CSS 范例

```css
# CSS Version: 2
div + p {
  background-color: yellow;
}
```

## HTML 范例

```html
<div>
  This area will not be styled.
  <p>This area will not be styled.</p>
</div>
<p>This area will be styled.</p>
<p>This area will not be styled.</p>
```

## 解释

用 `+` 来连接两个 *<u>element</u>* ，选择的是「紧跟着 *<u>element1</u>* 后面的 *<u>element2</u>* 」。也就是说，首先两个元素必须是并列关系，并且是紧接的并列关系。如上面的例子，在 `<div>` 里的 `<p>` 不会被选择，在 `div` 后的第二个 `<p>` 也不会被选择，只有紧跟着 `div` 后的第一个 `<p>` 被选择了。

---

# *<u>element1~element2</u>*

## CSS 范例

```CSS
# CSS Version: 3
p ~ ul {
  background-color: #ff0000;
}
```

## HTML 范例

```html
<div>This area will not be styled.</div>
<ul>
  <li>This area will not be styled.</li>
  <li>This area will not be styled.</li>
  <li>This area will not be styled.</li>
</ul>

<p>This area will not be styled.</p>
<ul>
  <li>This area will be styled.</li>
  <li>This area will be styled.</li>
  <li>This area will be styled.</li>
</ul>

<h2>Another list</h2>
<ul>
  <li>This area will be styled.</li>
  <li>This area will be styled.</li>
  <li>This area will be styled.</li>
</ul>
```

## 解释

用 `~` 连接的选择器与 `+ ` 最大的不同是，

- `+ ` 情况，只选择紧跟着 *<u>element1</u>* 后面的第一个 *<u>element2</u>* 元素
- `~` 情况，所有 *<u>element1</u>* 后面的 *<u>element2</u>* 都会被选择

如上述例子，在 `<p>` 后面，出现了两次 `<ul>` ，都会被选择。



