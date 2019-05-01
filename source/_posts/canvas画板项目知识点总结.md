---
title: canvas画板项目知识点总结
date: 2018-03-25 12:11:33
tags: 开发经验
categories: 学习
---

最近学习canvas，用其做了一个画板。画板支持画画，擦除，选色，以及保存导出等功能。最主要是通过这一个项目熟悉了canvas的API， 并且由于需要做移动触屏上的支持，所以对移动端的用户事件，移动端事件和PC端事件的区别有了更多的了解。这是项目地址： [Canvas-draw](https://sy2729.github.io/canvas-draw/.)

这个项目大概涉及到的知识点有：

1. 画板的鼠标事件：

(1)按下鼠标；
	`document.onmousedown`

(2)动鼠标；
	`document.onmousemove`

(3)松开鼠标；
	`document.onmouseup`

2. 如何通过一个变量作为开关触发和关闭点击状态，实现鼠标移动时作画；

3. 用div作画的缺陷，浏览器无法实时非常快速的监听事件，有延迟；

4. Canvas的缩放方式为等比缩放

5. API
    ```var ctx =  div.getContext('2d');
    ctx.strokeStyle;
    ctx.strokeRect;

    ctx.fillStyle;
    ctx.fillRect;


    ctx.clearRect;


    ctx.beginPath;
    ctx.moveTo
    ctx.lineTo
    ctx.lineWidth
    ctx.fill
    //会自动闭合
    ctx.closePath

    ctx.beginPath
    ctx.arc
    ctx.fill```


6. 常见的一个bug，很多人忽略了鼠标事件`clientX`和`clientY`所获取的坐标是相对于viewport（html）的坐标，canvas默认贴近viewport左上角，所以鼠标在canvas上的坐标就是在viewport上的坐标。但倘若一旦改变了canvas的边距，就会产生bug



7. 获取页面高度宽度API
`width: document.documentElement.clientWidth;`

`height: document.documentElement.clientHeight`

`document.documentElement.clientWidth` 这个 API 的作用是获取document宽度，也就是viewport的宽度

8. 移动端touch事件替代mouse事件：
```ontouchstart
ontouchmove
ontouchend```


9. 根据设备支不支持touch事件来决定监不监听touch事件 -- 特性检测

`div.ontouchstart = undefined` 表示不支持touch事件

`div.ontouchstart = null` 表示支持touch事件


10. touches支持多点触控，将所有触控点收集起来存放在touches中
所以移动端获取触控点的X、Y坐标时的API是
`e.touches[0].clientX`

11. 防止手机端出现滚动条或者页面在画画时滑动的办法： 
让canvas绝对定位；

12. canvas 保存画布为图片的方式是给一个a标签然后用canvas的toDataURL()API然后将canvas的整体存为一个字符串作为a标签的链接，紧接着再下载。

13. 用局域网进行快速调试：查看电脑端网络TCP／IP地址中的IPv4地址。