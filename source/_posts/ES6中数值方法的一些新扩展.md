---
title: ES6中数值函数的一些新扩展
date: 2018-01-09 15:25:13
tags: ES6
categories: 学习
---

ES6中对数值函数进行了新的扩充，比如:
* `Number.isFinite()`
* `Number.isNaN()`
* `Number.isInteger()`
* `Number.isSafeInteger()`
* `Math.trunc()`
* `Math.sign()` 会对输入进行数据类型的转换
* `Math.cbrt()`


## Number.isFinite()
`Number.isFinite()`用于判断输入的参数是否是一个无穷的数，即只要是一个有限的数都会返回值"true"（表示是）。

~~~~
Number.isFinite(13) //true
Number.isFinite('13') //false， 因为参数类型不是number
Number.isFinite(NaN) //false，同上
~~~~

## Number.isNaN()
`Number.isNaN()`用于判断输入的参数是否不是一个数字
~~~~
Number.isNaN(13) //false
Number.isNaN('13') //false
Number.isNaN(NaN) //true
~~~~

## Number.isInteger()
`Number.isInteger()`用于判断输入的参数是否是一个整数
~~~~
Number.isInteger(5) //true
Number.isInteger(5.0) //true
Number.isInteger('5.0') //false
~~~~


## Number.isSafeInteger()
`Number.isSafeInteger()`用于判断输入的数据是否在安全数值范围之类。对于系统的安全数值范围如果想要了解可以通过`Number.MAX_SAFE_INTEGER`和`Number.MIN_SAFE_INTEGER`的方法来获得。


## Math.trunc()
`Math.trunc()`是一个很有用的函数功能，要了解它的特别之处需要先了解`Math.floor()`,`Math.ceil()`,以及`Math.round()`这几个对于小数求整的方法，`Math.round()`也就是我们常说的“四舍五入”，而`Math.floor()`则是永远往数值小的那个方向的整数取值，无论是整数负数，而`Math.ceil()`则相反，往数值大的方向取值，如下：

~~~~
Math.round(4.6)  //5
Math.round(4.1)  //4
Math.round(-4.6) //-5
Math.round(-4.1) //-4
- - -
Math.floor(4.6)  //4
Math.floor(4.1)  //4
Math.floor(-4.6) //-5
Math.floor(-4.1) //-5
- - -
Math.ceil(4.6)  //5
Math.ceil(4.1)  //5
Math.ceil(-4.6) //-4
Math.ceil(-4.1) //-4
~~~~
而`Math.trunc()`则相反，有人称它的效果和参数为正数时的floor函数以及负数时的ceil函数一样，这样说可能有点绕，但是看了下面的例子应该就能明白了：
~~~~
Math.trunc(4.6)  //4
Math.trunc(4.1)  //4
Math.trunc(-4.6) //-4
Math.trunc(-4.1) //-4
~~~~
其实最好理解`Math.trunc()`的一个方法就是，不论正负，也不论小数部分是否大于0.5，用trunc函数求整得到的值永远等于原参数的整数部分。


## Math.sign()
`Math.sign()`用于判断参数的值为正数、负数或者0，返回的值为1，0，-1，NaN。
~~~~
Math.sign(-4) //-1
Math.sign(4) //1
Math.sign(0) //0
Math.sign(NaN) //NaN
Math.sign('4') //1
~~~~
注意，与之前函数不同的是，`Math.sign()`可以自动将参数的数据类型转换为number再对其进行判断。如最后一行代码的例子。

## Math.cbrt()
`# Math.cbrt()`函数可以求立方根，“cbrt”为“cube root”的缩写。