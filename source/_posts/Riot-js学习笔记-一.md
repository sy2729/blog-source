---
title: Riot.js学习笔记(一)
date: 2018-02-05 09:37:13
tags: Riot.js
categories: 学习
---
本来想集中精力学习Vue.js的，可惜这学期这门编程课要求使用Riot这个框架，加之选修了一些统计和数据方面的课程，Vue的学习不得不稍往后挪一挪。

Riot作为一个非常轻量级的框架，据说跟React的思维很像，但是体积非常小，压缩之后仅有几十K。虽然现在天下都是流行React，Angualr和Vue，但是在涉及小型项目时，Riot还是非常轻便和容易快速上手的，特别是在开发组件时。

但是网上关于Riot的教程都非常少，可能大家都对RAV趋之若鹜，再不济也会选择polymer的原因吧，其实我也不是很明白为什么老师指定我们必须使用这一框架。除了Riot的官方Doc之外，很少再有系统讲解的Riot使用或者预警其一些坑的教程。

我学习Riot的时候， 有以下一些心得总结：

* Riot的编译和常用框架的编辑一样可以分为浏览器的实时编译和本地预编译直接链接JS文件两种方式。实时编辑需要链接的Riot.js文件是附加了compiler的那个文件，而链接时链接的是riot的tag文件，如：
```
<script type="tag/riot" src="tags/main.tag"></script>
```
而index文件中最后需要加上`<script>`标签在其中使用`riot.mount("*")`来加载所有tag或者指定tag;

若要使用本地预编译的方式的话，需要先用npm安装Bower,再使用bower全局安装riot
```
npm install bower
bower install riot -g

#新建riot tag文件(若没有tags文件夹会新建)
riot --new tags/main.tag

#使用bower实时watch编译riot tag文件(同样的，没有的情况下会新建)，将main.tag编译成main.js
riot -w tags/main.tag js/main.js
```

* Riot中使用单花括号的模式来在HTML中运行当前tag的js代码（若习惯了其他框架的双花括号方式可在源代码文件中进行修改）

* Riot中的loop可直接在父级元素上声明，结果是会使需要被loop的object中的key暴露在父级元素的子元素中，这样就可以在子元素中直接对被loop的key和对应value值进行操作，如下：
```
<main>
<div each = { person }>
    <h1>{ name }</h1>
    <p>{ age }</p>
</div>

<script>
    this.person = {
        {
            name: "张三",
            age: 20
        },
        {
            name: "李四",
            age: 17
        },
        {
            name: "王老五",
            age: 24
        }
    };
</script>

<main>
```

简单的loop也可以直接通过以下的方式进行：
```
<main>

<p each={i in nameList}> { i }</p>

<script>
    this.nameList = ["张三","李四","王老五"]；
<script>
<main>
```
* Riot中省去了DOM绑定事件的操作，可以直接使用内置的事件，以下为常用的Riot事件列表（官网都没有的哦）：
{% asset_img riot_event.png %}

* Riot内置有元素的显示／隐藏功能，可以通过三种方式实现，分别是：“if/show/hide”:
```
<main>
    <h1 if={hideEl}>This is the Element to be hided</h1>
    <button onclick={change}>Click Me to Hide/Show the H1</button>

    <script>
        this.hideEl = False;
        this.change = function() {
            this.hideEl = !this.hideEl;
        }
    </script>
</main>
```
以上情况中，将if换成hide或show都能起到作用，只不过hide与show／if是反着的。

这里还需要注意一下show/hide/if区别的是，if值为false的话，整个元素是完全没有被渲染在DOM中的，而show／hide不论值如何都不会将元素从DOM中移除，只会在其属性上添加`display: none`或`display: block`以实现显示和隐藏的目的。




下一期Riot的学习笔记会总结关于Riot中各组件如何通信的问题，以及如何理解Riot中的生命周期。
