---
title: 【译】使用 currentColor 属性写出更好的 CSS 代码
date: 2016-09-21 15:44:06
tags: [CSS, 前端开发, 翻译]
---

[原文地址](https://hashnode.com/post/writing-better-css-with-currentcolor-cit5mgva31co79c53ia20vetq)

有一些极其强大的 CSS 属性有着很好的浏览器支持，但是开发者们却极少用到。`currentColor` 就是其中之一。

MDN defines currentColor as

在 MDN 上对 currentColor 是这么[定义](https://developer.mozilla.org/en/docs/Web/CSS/color_value#currentColor_keyword)的。

> The currentColor keyword represents the calculated value of the element's color property. It allows to make the color properties inherited by properties or child's element properties that do not inherit it by default.

> `currentColor` 关键字表示了元素的颜色属性的计算值。它可以使元素的属性中使用颜色的时候继承本身的 `color` 属性，或者子元素可以从父元素继承颜色属性。简言之，它改变了元素的默认继承属性。

In this article, let's get an overview on how to use CSS currentColor keyword in some interesting ways.

在这篇文章里，我们来看看如何来使用 CSS 的 `currentColor` 关键字。有很多很有趣的方式。

## 简介

`currentColor` 关键字获取 `color` 属性中的颜色值，把它保存在自身的一个规则里。

当你想继承使用 `color` 属性中的颜色值的时候，就可以用 `currentColor` 关键字。如此一来，当你修改了 `color` 属性的值，所有用 `currentColor` 关键字来引用这个颜色值的地方都会相应的自动修改。是不是很棒？

``` css
.box {
    color: red;
    border 1px solid currentColor;
    box-shadow: 0 0 2px 2px currentColor;
}
```
在上面的代码中，可以看到我们没有到处重复使用相同的颜色值，而是使用 `currentColor` 关键字。这样带来的好处就是使得 CSS 变得更好维护了，我们不需要再到处去维护颜色值了。

## 可能的用法

让我们来看看一些 `currentColor` 的用例和例子。

### 简化颜色定义

像链接，边框，图标和阴影经常会使用和他们父元素相同的颜色，用 `currentColor` 值来表示颜色值避免了在很多地方去用各自的颜色值，从而提高代码的可维护性。

举个例子：

``` css
.box {
    color: red;
}
.box .child-1 {
    background: currentColor;
}
.box .child-2 {
    color: currentColor;
    border 1px solid currentColor;
}
```
在上面的代码段中，可以看见没有给 `background`、`border` 属性指定某个颜色，而是用 `currentColor` 来代替。这样一来，它们就都是红色了。

### 简化过渡和动画

`currentColor` 能让过渡和动画变得更简单。

让我们来看一下之前的一个例子，增加改变悬停的颜色的效果。

``` css
.box:hover {
    color: purple;
}
```

在这里，没有在 `:hover` 里重新定义三个颜色值，而只是修改了 `color` 属性的值。所有使用了 `currentColor` 的属性都会在有悬停的时候自动应用这个修改。

### 在伪元素中的应用

类似 `:before` 和 `:after` 这样的伪元素也会从它们的与元素继承 `currentColor` 的值。这可以应用在诸如有动态颜色的提示（tooltips），覆盖层（overlays），还可以给它加上一个具有透明度的效果。

``` css
.box {
    color: red;
}
.box:before {
    color: currentColor;
    border: 1px solid currentColor;
}
```
在这段代码里，`:before` 伪元素继承了父元素的颜色值，将父元素的值用在了 `color` 和 `border-color` 上，用这个机制，可以实现像 tooltips 之类的东西。

### 和 SVG 配合使用

SVG 也可以从父元素里获取 `currentColor` 的值。当在不同的地方使用这个 SVG 的时候，如果想要直接继承所在父元素的颜色而不想每次都都指定，这真的是一个很有用的方法。

``` css
svg {
    fill: currentColor;
}
```
在这个例子中， svg 和它的父元素会有相同的填充色，也会跟着父元素的颜色的修改而动态变化。

### 和渐变配合使用

`currentColor` 还可以用在渐变中，如果渐变中的一种颜色是父元素的颜色的话，是很方便的。

``` css
.box {
    background: linear-gradient(top bottom right, currentColor, #FFFFFF);
}
```
在上面的例子中，顶部的渐变色总是和父元素的颜色相同。虽然在这个例子中只能使用一种颜色来动态赋值，但不得不承认这是一个很巧妙的技巧来生成基于父元素颜色的动态渐变。

这里有一个 [Codepen 上的例子](http://codepen.io/alkshendra/pen/xEVrJJ?editors=1100#0)，演示了上面所有的用例。

## 浏览器支持

CSS 的 `currentColor` 属性是从 SVG 细则中引入 CSS3 的，其实它 2003 年就在存在了。因此，对于 `currentColor` 的支持已经十分广泛了，IE8 及以下版本是个例外。

下面这张图表是从 [caniuse.com](http://caniuse.com/#feat=currentcolor) 网站查询得到的：

![currentColor 浏览器支持](https://res.cloudinary.com/hashnode/image/upload/v1474021764/g03f4hx1ftb0frtoonfw.png)

## 总结

CSS 的 `currentColor` 属性是一个未被充分使用的强大特性。它的支持性很好，并且带来了更多的方式来让我们的代码变得更清晰。

由于 CSS 变量还在试验阶段，习惯使用 `currentColor` 属性绝对对你是很有帮助的。
