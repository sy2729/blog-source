---
title: Parcel踩坑记
date: 2018-08-15 23:54:06
tags: Parcel 前端工程化
categories: 学习
---
最近做公司的landing page，因为才学了模块化不久，用MVC的方式把每个组件分成单个JS文件来写，刚开始思路的确非常清晰，可到得后面，JS文件多起来，网页开始以肉眼可以观察到的速度变得慢了起来（当然也有引用的媒体文件增多了的原因）。于是在试了下用Parcel将index.js文件作为入口引入别的模块文件之后，速度的确是可以感知的变快了，我觉得并不是心理作用，看来减少http请求的确是有帮助的。

我担心自己用了Parcel就再也静不下心来学习webpack了，因为深知配置webpack耗费的可不是一两个小时而是整整一段时间，不然也不会有webpack工程师一说。不过parcel真的很好用。遇到一些坑，除此之外parcel真的非常容易上手。现总结如下：

1.Parcel中使用jQuery在编译完成之后浏览器中会显示’$’undefined。
* 原因是parcel使用了闭包所以导致jQuery无法将’$’赋值到window上
* 解决办法：
    * 使用CDN - 避免对jQuery的编译
    * 如果仍然想使用本地jQuery而不使用CDN，那么使用parcelignore在编译时忽略jQuery
		如node_modules/jQuery // Ignore this entirely

2.Parcel中如果在使用inline-style中引用到了文件，如background-image:url()， 并且在别处的CSS文件中引用了同一文件时，会出现加载资源错误，因为parcel在编译CSS（如果将CSS引入JS中）时会将了图片作为dependencies处理，改变文件的后缀名，CSS中的引用会跟随改变引用名，所以CSS的引用的图片是不会加载错误的，但是好像parcel没有探测到inline-style中的文件引用，所以文件名不会改变，所以就会加载错误。

3.Parcel在build打包时，如果直接build好像会出现文件资源加载路径出错，需要使用“--public-url ./“ 来打包，所有的文件会放在一个dist（默认）的文件夹中。如果要改变文件夹名称，使用 “-d xxxx”来改变。
在正常开发环境时，只需要parcel index.html 这么简单两个单词的命令就可以打包了，并且parcel会自动监测文件的变动并进行模块的热加载。但是需要注意，需要使用parcel返回的那个local网址和端口，否则文件路径会显示错误。