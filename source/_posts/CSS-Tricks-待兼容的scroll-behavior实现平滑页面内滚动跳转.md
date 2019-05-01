---
title: '[CSS-Tricks]待兼容的scroll-behavior实现平滑页面内滚动跳转'
date: 2018-03-05 12:27:57
tags: CSS
categories: 学习
---

在CSS-Tricks上看到一篇文章，才知道竟然有scroll-behavior这一属性，可以实现子元素内容超出父元素高度时用`overflow: scroll` + `scroll-behavior: smooth` 实现在页面内通过`a`标签的`href`链接进行跳转的平滑滚动效果。

这一效果的通常使用场景有二，一是页面内某特定区域自成一体，有多重段落和内容，顶部（一般为顶部）有链接可以跳转到各段落区域；另一场景即是最常见的单页landing paging。很多常见的产品展示页或者个人主页甚至小型公司都只有一个页面，通过链接在页内跳转实现多个页面的错觉。这种方式通常也很简洁大方。不过一般需要通过JS实现和控制滚动效果和速度。

CSS-Tricks上的这篇文章列举了第一个场景的使用方式 - [scroll-behavior - by  GEOFF GRAHAM](https://css-tricks.com/almanac/properties/s/scroll-behavior/)，我试了下，其实全页也可以实现，只是`scroll-behavior`在实现时需要注意的是：第一，要声明父元素的高度宽度；第二，父元素本身要使用`overflow：scroll`，也就是说，在实现全局页面平滑滚动时，在正常文本撑开body元素导致的scroll效果中，在body上使用`scroll-behavior`是没有效果的，而不知为何，body即使使用了`overflow：scroll`也没有产生效果，所以需要在html上使用`overflow：scroll`，`height: 100vh`和`width: 100%`，即可以得到全局平滑滚动的效果了。例子详情可见[scroll-behavior - CodePen](https://codepen.io/shuaiyuan/pen/JpQjao)

但是很不幸的是，目前这一特性的浏览器兼容性不是特别高，
{% asset_img compatibility.png %}

所以期待至少Safari能尽快赶上步伐。