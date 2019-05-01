---
title: 简易NavBook导航网站实现及知识点总结
date: 2018-03-22 13:18:16
tags: 开发经验
categories: 学习
---

最近做的小项目是一个导航网站，用户可以自定义编辑快捷键对应的网址- [Navbook](https://sy2729.github.io/navbook/)。实现过程中大概运用到的知识点有：


1.100vh 指屏幕（视口）高度;

2.浏览器访问页面icon的方式是访问网址根目录下的favicon.ico;例如www.qq.com/favicon.ico

3.403 forbidden


* 如何通过循环不断生成相互嵌套的div
* 如何监听键盘事件并通过键盘址打开hash中对应的网址；
* 如何点击对应键盘中的按钮修改hash中对应键盘储存的网址；
* 如何利用localStorage存储用户设置；
* 如何询问用户并获取用户输入；
* 如何获取各个网站的fivcon；
* 如何捕获错误事件，并为之准备一个备用fivcon
* 如何更好的封装函数

