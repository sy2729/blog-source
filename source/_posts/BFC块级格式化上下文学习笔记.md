---
title: BFC块级格式化上下文学习笔记
date: 2018-03-17 10:29:44
tags: CSS
categories: 学习
---

跟定位，浮动，布局等问题类似，CSS中除了这些难点重点之外还存在一个常被面试官提起的知识点——BFC。但是对此很多人存在争议，BFC真的是重点吗？从我学习的感受来看，BFC由于建立在对浮动所带来的副作用的理解基础上，目前只能算难点，但是实际应用来看，由于其自身也会带来额外的副作用（除非用并未得到所有浏览器支持的属性`display: flow-root`实现），其实用价值并不足以使其成为重点。但无论如何，它都是面试的一个重点。

了解BFC的主要两点：
## BFC定义
##### CSS规范中对 BFC 的描述
```
9.4.1 块格式化上下文

浮动，绝对定位元素，非块盒的块容器（例如，inline-blocks，table-cells和table-captions）和'overflow'不为'visible'的块盒会为它们的内容建立一个新的块格式化上下文

在一个块格式化上下文中，盒在竖直方向一个接一个地放置，从包含块的顶部开始。两个兄弟盒之间的竖直距离由'margin'属性决定。同一个块格式化上下文中的相邻块级盒之间的竖直margin会合并

在一个块格式化上下文中，每个盒的left外边（left outer edge）挨着包含块的left边（对于从右向左的格式化，right边挨着）。即使存在浮动（尽管一个盒的行盒可能会因为浮动收缩），这也成立。除非该盒建立了一个新的块格式化上下文（这种情况下，该盒自身可能会因为浮动变窄）
```


## BFC功能
（1）管理内部子元素
		让不受管理（包不住）的子元素接受管理
		让父元素浮动／绝对定位／变成不是block的block（inline-block、table-cell，table-caption）／非overflow：visible／display：flow-root(最新标准，作用是仅处罚BFC且不带来任何多余效果，但浏览器支持有限)

(2）使子元素们的竖直margin合并

	*但是此BFC不会管其子元素创建的新BFC中的子元素（包不住孙元素），	如果BFC2不管孙元素，那么BFC就要管孙元素，反之则反，例如：

		`
        BFC
		  -子元素／BFC2
			-孙元素
        `
（3）让内部元素和外部元素划清界限／兄弟之间划清界限

*总结：BFC能和清除浮动实现一样的效果，但是可能带来额外的副作用，而不带来副作用的纯制造BFC的样式`display： flow-root`目前又存在浏览器兼容问题。所以BFC是时代的产物，一般情况下不用。在某些情况下，当子元素想要穿透父元素时，也可以用border-top属性来替代BFC


