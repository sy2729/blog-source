---
title: 实现类JQuery的简单封装
date: 2018-05-15 22:00:12
tags: Javascript
categories: 学习
---

原来JQuery只是一个构造函数！
实现JQuery只需要写好一个函数，把所有JQuery的方法封装进去！
封装进去后只需要用JQuery函数获取参数就可以使用所有方法！

1.首先在全局创建JQuery函数：
```
window.JQuery = function(selector) {

}
```

这个selector即将作为我们之后常见的选择器或者实现‘链式操作’的先决条件；


2.判断这个用户给予的input是否是一个selector或者已经是一个Node
```
window.jQuery = function(nodeOrSelector) {
    let node = {};
    if (typeof nodeOrSelector === 'string') {
        let temp = document.querySelectorAll(nodeOrSelector)
        for(let i = 0; i < temp.length; i++) {
            node[i] = temp[i];
            node.length = temp.length; 
        } 
    } else if (nodeOrSelector instanceof Node) {
            node = {
                0: nodeOrSelector,
                length: 1
            }
        }
}
```

如果是一个选择器，那么调用原生DOM的API选择元素并且放到伪数组中node中；
如果是一个Node，那么为了保持return object的一致性，也强行把它转化成一个伪数组, 这一点实现了我们的链式操作；

3.接着为node添加各种自定义的API，在这里，也就是我们自己写的JQuery API。
```
window.jQuery = function(nodeOrSelector) {
    let node = {};
    if (typeof nodeOrSelector === 'string') {
        let temp = document.querySelectorAll(nodeOrSelector)
        for(let i = 0; i < temp.length; i++) {
            node[i] = temp[i];
            node.length = temp.length; 
        } 
    } else if (nodeOrSelector instanceof Node) {
            node = {
                0: nodeOrSelector,
                length: 1
            }
        }

    node.addClass = function(classes) {
            for (let i = 0; i < node.length; i++) {
              node[i].classList.add(classes)
            }
        }

    node.setText = function(text) {
        for (let i = 0; i < node.length; i++) {
            node[i].textContent = text;
        }
    }

}
```

4.最后一步，怎么用呢？需要先把node return出来给用户使用啊
```
window.jQuery = function(nodeOrSelector) {
    let node = {};
    if (typeof nodeOrSelector === 'string') {
        let temp = document.querySelectorAll(nodeOrSelector)
        for(let i = 0; i < temp.length; i++) {
            node[i] = temp[i];
            node.length = temp.length; 
        } 
    } else if (nodeOrSelector instanceof Node) {
            node = {
                0: nodeOrSelector,
                length: 1
            }
        }

    node.addClass = function(classes) {
            for (let i = 0; i < node.length; i++) {
              node[i].classList.add(classes)
            }
        }

    node.setText = function(text) {
        for (let i = 0; i < node.length; i++) {
            node[i].textContent = text;
        }
    }
    return node

}
```

5.最最后一步，'JQuery'几个字写起来太麻烦，所以在全局变量中用‘$’取代'JQuery'
window.$ = JQuery;

所以之后的调用就可以先用$('div')选中元素再使用方法了！
$('div').addClass('red')

所以JQuery最最基本的封装原理就是这样。

## JQuery 源码精髓： JQuery接受一个旧的节点 （或者字符串CSS选择器），反回一个新的对象，这个对象当中带有所有JQuery的新的API （JQuery实质上是一个函数）.
