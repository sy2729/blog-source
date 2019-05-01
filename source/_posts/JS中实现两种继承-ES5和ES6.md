---
title: JS中实现两种继承-ES5和ES6
date: 2018-09-17 16:26:16
tags: JavaScript
categories: 学习
---

JS本来不像Java这样的语言有class和继承这种特性的，JS有自己的类似于实现继承特性的语法功能，叫做原型链。后来为了让JAVA程序员也能快速理解并上手JS，ES6推出了JS中的Class，所以在此总结下新旧两版中分别实现继承的方法。

首先，以一段代码为例：
```
function Human(name){
     this.name = name
 }
 Human.prototype.run = function(){
     console.log("我叫"+this.name+"，我在跑")
     return undefined
 }
 function Man(name){
     Human.call(this, name)
     this.gender = '男'
 }

Man.prototype.fight = function(){
	console.log('糊你熊脸')
}
```

现在如果我们要实现`var man1 = new Man()`中man1具有Human的属性，需要实现Man构造函数对Human构造函数的继承。那么：
在ES5中的写法：
```
Man.prototype.__proto__ = Human.prototype
```

只需要一句话，将man1的构造函数Man的原型链接到Human构造函数那里就可以了，这样，man1就可以使用Human的属性，也就是’run‘这个函数。

但是此法在IE中有bug，IE不支持用户直接操作`__protp__`属性，因此，有一个巧招：
```
var f = function(){}
f.prototype = Human.prototype
Man.prototype = new f()
```
直接声明一个新的空构造函数，将此构造函数的prototype设置为和Human的一样（这样做的原因是为了让这个f函数也能构造出Human函数中构造出的公有属性），然后将Man的构造函数设置为由f构造出来的函数，从而在原来的最初Object构造函数之前，插入一个f。

进一步说，因为要做到`Man.prototype.__proto__ = Human.prototype`,也就是`Man.prototype.__proto__ = f.prototype`，但是又不能直接操作`__protp__`，因此可以用new来实现，因为在`var obj = new Fn()``new`默认会将右边构造函数的prototype赋值到左边的`__protp__`上。但是为什么不直接`new Human()`呢？因为New不仅会默认做上述的步骤，还会做这些：
    1.产生一个空对象
    2.this = 空对象
    3.this.__proto__ = Fn.prototype
    4.执行Fn.call(this, x, y, z ...)
    5.return 4 的结果
问题就出在4这一步，Human这个函数上除了构造公有属性外，还有自身的属性name，那么`new Human()`会导致其自身的私有属性也跑到Man的原型链上去，虽然不会造成什么问题，但是这是我们不需要的。

ES6写法：
```
class Human{
     constructor(name){
         this.name = name
     }
     run(){
         console.log("我叫"+this.name+"，我在跑")
         return undefined
     }
 }
 class Man extends Human{
     constructor(name){
         super(name)
         this.gender = '男'
     }
     fight(){
         console.log('糊你熊脸')
     }
 }
```
用class表示一个构造函数，用constructor储存自身属性／私有属性，用函数形式储存公有属性。
要做到一个构造函数继承另一个构造函数，只需要两步：
    1.用extends；
    2.在constructor中用super继承父类的this和变量。

但是用class的一个缺点是如果想在原型链公有属性上声明一个字符串、数字等数据类型的话无法声明，因为属性全是用方法function表示的，如fight()。所以另一个巧招是：
```
class Man extends Human{
     constructor(name){
         super(name)
         this.gender = '男'
     }
     get 喉结(){
         return true
     }

     fight(){
         console.log('糊你熊脸')
     }
 }
 ```

 