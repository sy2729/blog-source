---
title: 兼容IE6的一些Hack
date: 2018-03-13 20:17:27
tags: 浏览器
categories: 学习
---

有前端工程师称自己可以不用框架从主流浏览器一直写代码到兼容到IE6，这的确是很吊了。在这个框架横行的年代，连大多数框架都放弃了IE6-8，还在为各政府及大型国企，公立机构等写网站的工程师确实不得不忍受IE浏览器。不过作为小菜鸟，目前我只稍微了解了一些常见的IE8一下的兼容方式。

### 1.IE6双倍边距

产生因素：当元素有float属性，又有margin属性时，在IE6下面显示的margin的值是设置值的两倍。

解决方法：将有float属性的元素添加display:inline属性。

### 2.伪类选择器hover

产生因素：IE6只支持a标签的CSS定义hover效果，其他标签添加hover效果没有任何作用。

解决方法：一方面可以使用JavaScript添加鼠标移入效果，另一方面只能将其他标签改变为a标签后再添加hover效果。

### 3.定义元素的不透明度

产生因素：opacity:0.5，可以改变元素的透明度，取值范围是0~1，但是IE6不支持这种透明度表现方式。

解决方法：IE6浏览器专属的透明度属性， filter：alpha（opacity=80），取值范围是0~100。

### 4.IE各个版本hack

    属性hack方式

.box {_width:100px;}             /* IE6专用*/

.box {*+width:100px;}          /* IE7专用*/

.box {*width:100px;}            /* IE6、IE7共用*/

.box {width:100px\0;}           /* IE8、IE9共用*/

.box {width:100px\9;}           /* IE6、IE7、IE8、IE9共用*/

.box {width:330px\9\0;}        /* IE9专用*/

    选择器hack
    
*html .box{width:100px;}       /*IE6*/ 

*+html .box{width:100px;}     /*IE7*/ 