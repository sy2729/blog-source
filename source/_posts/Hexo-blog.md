---
title: Hexo博客搭建
date: 2018-02-25 12:56:29
tags: Hexo
categories: 学习
---
简单分享下我是如何搭建Hexo博客的

首先安装node.js这是必须的。

然后用命令行全局安装hexo脚手架`npm install -g hexo-cli`

接着进入你想在电脑上存放博客资料的一个文件夹地址，将这个地址变为一个Github repo。有两种方式，一是在Github上新建好了repo然后clone到本地，二是在本地`git init`新建repo然后push到远程。

然后在这个目录中新建hexo项目`hexo init myBlog`, myBlog只是项目名称，可随意更改

进入myBlog，`npm i`

新发表一篇文章试试：`hexo new XXXX`,这个XXXX是新的文章的名字

接着可以在当前一个叫做_config.yml的文件中修改博客的一些属性和标签，包括名字，作者，使用什么什么插件，主题，github repo的地址是什么等等。

最重要的是安装部署插件`npm install hexo-deployer-git --save`,这样才能通过命令行直接一键更新网页上的博客内容，使其与本地同步。

安装完成之后，试着上传（部署）自己的第一篇博客，`hexo deploy`。

然后再到github pages中去开启这个repo的page功能，使这个page变成自己github博客仓库的展示页，也就是我们博客的正式地址。

如果更新了博客，有时候`hexo deploy`之后网页端没有反应，需要先使用命令`hexo clean`清除再进行`hexo deploy`。其实deploy这个命令也可以简写为`hexo d`。
