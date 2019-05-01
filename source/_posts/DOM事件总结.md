---
title: DOM事件总结
date: 2018-06-13 13:18:32
tags: JavaScript
categories: 学习
---
DOM定义的发展经历了几代，第一代称为DOM0:

- DOM 0 
    如何绑定事件：
	btn.onclick =  function() {}
	btn.onclick.call(btn, arguments…)

第二代称为DOM1
- DOM 1
	升级汇总前一阶段

- DOM事件升级纪年
DOM 1 - 2000
DOM 2 
DOM Event事件的最新标准是DOM2中的标准

DOM1中绑定事件:
在HTML中
`<button id = X onclick = 'print()'> A </button>`
onclick = '要执行的代码'
一旦用户点击，浏览器会用eval来执行等号后的代码，eval(print())

在JS中
X.onclick = print
因为X.onclick对应一个函数，所以右边只需要是一个函数即可

DOM2:
xxx.addEventListener('click', function() {})

两者区别：
DOM1中，由于onclick是一种属性，那么它是唯一的，所以无法给onclick绑定两个属性

DOM2中：
addEventListener建立的是一个队列
移除队列中的function，用removeEventListener('click', function名字)
前提是知道function的名字，而非使用匿名函数

用removeEventListener实现一次事件监听，这也是`one`监听一次事件的实现原理


# 事件模型 捕获 | 冒泡

假如三个div嵌套，从外到内的div分别类比成爷爷，爸爸，儿子，那么点击儿子的时候，如果使用`addEventListener`API, `El.addEventListener('onclick', function(){})`不传第三个参数（不传第三个参数JS默认帮我们传false），那么点击事件函数触发的方式分别是从内到外，那么分别绑定在三代上的事件触发的顺序就是由内到外；如果第三个参数传参数为true的话，函数的触发顺序分别是从外到内。

但是有个特例，如果是被点击的元素本身同时有两个事件（分别传入了true和false——声明了想要选择冒泡或者捕获模型），那么事件函数执行的顺序是按照函数注册（书写）的顺序触发的。 

# 事件捕获冒泡用例

事件的捕获冒泡模型的最佳应用场景应该就是在点击页面其余地方关闭浮动窗口了。
这是用户体验的一大进步，用户永远不用再心累的一定要精准地点到那个右上角小叉叉才能关闭浮动窗口，而是轻松地往窗口外任何一个地方一点就能关闭窗口。
实现这个效果的基本原理就是监听整个页面——document，只要点到它身上时就关闭窗口。
但是问题在于就算点到document里的触发窗口的button，其本质也算点到document，就像我打你的鼻子，也是在打你一样。那么最后事件一定会从button起往上冒泡触发绑定在document上的关闭窗口事件，那么窗口刚因为button被点击打开了就又因为document被点击给关闭了。
所以这个时候需要在触发button是阻止事件的冒泡：e.stopPropagation();

但是这样有一个缺点。如果页面的浮动窗口过多，那么关闭浮层所需要在页面监听的事件也越多，会浪费内存。所以最好是使用JQuery中的one功能，如：
		```
			$(button).on(‘click’, function(0 {
				$(‘pop’).show();
				$(document).one(‘click’, function() {
					$(‘pop’).hide();	
				})
			})

			$(浮层和button的父元素).on(‘click’, function(e) {
				e.stopPropagation();
			})
		```

    为什么需要上面代码中的e.stopPropagation()部分？
    因为事件会冒泡，添加在document上的事件即使在button点击后才被添加，但是点击之后触发的顺序是：浮层显示同时添加事件监听给页面；浮层显示结束后开始冒泡，此时事件监听添加已经完成，所以在冒泡到document时事件已经被绑定上去了，一样会触发事件，所以不用e.stopPropagation()一样没有效果。

但是除了以上寻找button和document中间元素使用e.stopPropagation()的写法，更有效的方法是：

```
			$(button).on(‘click’, function(0 {
				$(‘pop’).show();
				setTimeout(function() {
					$(document).one(‘click’, function() {
						$(‘pop’).hide();	
					})
				}, 0)
			})
```