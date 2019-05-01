---
title: 常见的一些页面JS特效分析
date: 2018-04-05 16:45:46
tags: JS
categories: 学习
---

最近还是在用原生JS写特效，Jquery虽好，但是原生的玩转了才能在将来用Jquery叱咤风云。

常见的页面特效这次介绍几个：loading animation（加载动画）， 导航栏的跟随变化，导航栏的高亮，页面平缓滚动的特效，导航栏的鼠标hover，以及常见的程序员简历各种技能栈加载时的技能条加载特效。

### Loading Animation
之前一直以为loading animation是多么高大上的事情，需要在服务器端或者后台获取什么数据加载完成与否的信息。后来发现，完全可以在浏览器端巧妙的解决这个问题。理解Loading Animation需要对网页的页面渲染有一个基本的了解。其实页面各种资源加载完成与否某种程度上就是在于html是否从头run到了尾。那么在body头部插入加载动画，遮住背后还未加载完成的各种信息，再在页面body的最后一个标签后加上script从DOM中移除这个loading元素就可以了。这种的逻辑比较巧妙：如果页面已经运行到了我移除animation的位置，那么是否意味着前面的标签也加载完毕了呢？那么这个时候移除animation是非常顺理成章的。

### 导航栏的跟随变化
很多fancy网页中都有导航栏的跟随功能，也就是页面打开时导航栏跟页面是融为一体的，而当用户滑过一个临界点的时候突然粘在了页面顶部，其实这个和之后很多的特效原理一样，都是通过时刻感知用户的滑动事件获取当前的视口（视窗）顶部距离页面顶部来决定的。

感知滚动的两种事件：onscroll, onwheel

接下来在事件回调函数中获取距离页面顶部滑动的距离，用window.scrollY，如果超过一定的距离，那么就给navbar加上一定的class属性来改变其特性。

### 导航栏的点击和高亮

导航栏的每个分项hover时下面有一条线高亮，这个既可以通过CSS做也可以用JS做，但是JS做有更多的可控性。
所以用JS的mouseenter和mouseleave事件监听器

##### target & currentTarget

在用户事件监听的callback回调函数中，又常会遇到这个问题———我是用target这个属性来获取DOM元素好呢还是currentTarget好呢？

太多的文章讲target & currentTarget了，我就简单一句话总结一下，

用户操作的元素： target
我们监听的元素： currentTarget

意思就是用户点击的是哪个，那target获取到的就是哪个(就是用户最精准的点击到的那一个，只有那一个)。因为不要忘了如果用户点击一个div里面的button里面的span，他形式上实际是同时点击到了这三个元素的，而target获取的是当前点击的最底层的那个；

currentTarget简单明了，你这个监听器装在哪个元素上，我就获取那个，即使点击到监听的这个元素内部的别的元素，我也只给你返回这个被监听的元素。

(但是mouseenter事件不会有这两种区别，常见于click等事件)

##### 如何获取不同元素——子元素／兄弟元素／父元素等

获取子元素: `element.children`
获取父元素: `element.parentNode` & `element.parentElement`
获取兄弟元素: 很多时候获取兄弟元素浏览器有一个“bug”。

在使用nextSibling的时候可能获取到的是两个element之间的回车 -- 解决办法：递归
let brother = a.nextSibling;
while(brother.nodeType === 3) {
	brother = brother.nextSibling;
}

(或者把brother.nodeType === 3换成brother.tagName === 想找到的tagName（必须大写）)

目前有document.nextElementSibling API可以解决这个问题，但是IE8和Safari存在兼容问题



### 页内跳转

页内跳转我之前的博客写过一篇关于CSS的用法，但是浏览器兼容目前还存在比较大的问题，所以暂时应用场景很窄。JS实现的页面平滑跳转有很多可塑性，包括跳转的速度，跳转到的高度等都是可以控制的。

页内跳转在使用a标签锚点ID进行跳转时有可能因为页面顶部浮动的navbar遮挡住跳转到的标签的顶部，解决这个“bug”的办法是可以在跳转到的元素上用padding把元素在页面上的定位伪装一下，但是用JS来解决更佳。

通常需要先阻止默认的跳转行为，用e.preventDefault(),接着获取每次点击时当前被点击的标签的href（但是这里有bug，如果直接用a.href获取，浏览器会默认解析href，加上http协议，所以应该使用a.getAttribute('href')），接着再获取应跳转到的那个元素距离整个页面顶部的距离（用element.offsetTop获取，注意：用element.getBoundingClientRect获取到的距离是标签相对于视口，也就是当前所看到的顶部的距离，这个距离会随着滚动一直变化，所以在此情景下不适用），再用window.scrollTo()让页面滚动到标签距离页面的距离左右的位置


### 获取当前所在区域并高亮nav

* 可以用data-xxx来标注一类标签并用querySelector['data-xxx']来获取
* 比较当前页面距离页面顶端的距离和每一个元素距离页面顶端的距离大小，算出最小那个，获取其id来找到对应的navbar，接着再高亮navbar

* 解决滑动到对应位置navbar高亮和hover navbar高亮都加active class冲突的问题——用不同的class

### 让元素一开始就浮动起来的原理
很多library可以实现这个scroll-and-reveal的效果，其实原理很简单，就是将监听scroll事件获取的视口距离页面高度和获取的元素距离视口高度俩参数封装起来，再让用户输入需要实现此效果的元素。其背后的原理就是：
先让元素一开始带上offset class，在移动到它附近的时候移除这个offset class（在offset这个class上写各种CSS特效）

#给技能栈progress bar加上animation的方法，同上