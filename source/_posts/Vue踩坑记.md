---
title: Vue踩坑记
tags:
  - 踩坑记
  - Vue
categories:
  - 学习
toc: false
date: 2018-09-21 21:51:17
---

这里面每个坑都至少花了我一个小时才figure out😂，血泪的教训

* `Props`的命名不要出现camelCase
* `$emit` 的事件名最好用-的形式

* 本地图片引用要么放在public里用‘/’绝对路径来引用，要么用require来引用


* nom run build 打包后index空白，路径不对的问题：
    * 在vue-cli 2中：网上有很多教程
    * 在vue-cli 3中: 直接在根目录添加`vue.config.js`，然后
```
module.exports = {
    baseUrl: './',
}
```



* Ant Design Vue这个UI框架中，教程有的部分真的TM写得屎烂，明明有些名称是class样式，写尼玛写成组件的形式放在展示的代码里。我艹你大爷。我还感谢你写个框架。哈。哈。哈。哈。

* 在使用Vue transition时如果想要使用transform等属性时不起作用，多半是组件内部的transform属性优先级更高，在声明vue transition的属性是给`transform`加上`!important`就能解决这个问题