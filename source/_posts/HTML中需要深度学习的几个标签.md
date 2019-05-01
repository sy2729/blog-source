---
title: HTML中需要深度学习的几个标签
date: 2018-03-11 14:03:59
tags: HTML
categories: 学习
---
HTML很多人都说简单，但是有几个标签并没有看起来这么简单，其中的form和table是很多自学者初学者都会跳过或者浅尝辄止的标签。

1.button 和 input button的区别，input button中要指定button内部的文字，文字是写在元素内的，通过`value = “”`实现；

2.Chrome中禁用Javascript的方式，如何操作？

3.`span` 默认inline, `image`默认inline（一种特殊的inline），`div`，`ul`，`hi`默认`display：block`

4.可替换元素宽高不知道，由内容的宽高决定。外观渲染独立于CSS。可用属性width和height控制，更可用CSS来覆盖属性width和height

5.在空窗口打开链接： `<a href=“url” target = “_blank”></a>`

6.神奇的`contenteditable`

7.表示搜索框的`<input type=“search”>`,
表示slider的`<input type=“range”>`

8.iframe 也是可替换标签
同一页面内通过a标签在iframe内实现跳转，只需要在iframe中加入`name`属性，在a中加入`target`属性

9.a标签中target属性的四个值及对应意义：
`target=“_blank”`: 在空页面打开
`target=“_blank”`: 在自己页面打开
`target=“_blank”`: 在父页面打开
`target=“_blank”`: 在顶层页面打开（结合嵌套的iframe理解）

10.由`a`标签`download`属性牵扯出的文件下载问题。有两种下载方式，第一种是http协议中写的`content-type：application/octet-stream`时，默认下载；第二种是http仅是`content-type: text/html`时，使用a标签添加属性download来进行强制下载；

11.`a`标签中`href`属性的值如果不指明使用何种http协议，那么打开的网页即使用何种协议；

12.`a`标签中`href`属性中链接如果是#开头是不会发出http请求的，因为是页面内的跳转；如果是`” “`空字符串，会进行自身页面刷新

13.`a`标签中的`href`可以写伪协议——javascript代码——历史遗留问题；但是它可以用来避免页面跳转：`<a href=“javascript:;”></a>`


14.`a`标签跳转页面发起GET请求；form标签跳转页面发起POST请求(需要声明method=“post”)；

15.如果`form`标签中没有提交按钮，则无法提交这个form；

16.用户提交表单的时候http协议中的data遵循www-form-urlencoded的语法，英文之外的语言会被自动转译为Unicode. input的name会自动成为http协议中第四部分的key值。

17.如果将表单的method写成了get，那么表单数据会默认成为查询参数，而method为post的情况中表单数据会出现在http协议的第四部分中。而Post支持手动添加查询参数，但get永远不支持添加第四部分内容

18.`form`标签的`target`使用规则和表现行为跟`a`标签一样。

19.如果一个form中只有一个button，那么会自动升级为提交按钮（submit）。（submit类型的button可以回车提交）

20.所有没给name的input服务器是拿不到值的；

21.`table`标签中的子标签顺序无关紧要，`colgroup`的用法比较厉害；`border-collapse: collapse`经常搭配使用;


