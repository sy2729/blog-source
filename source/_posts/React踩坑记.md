---
title: React踩坑记
tags:
  - React
  - 踩坑记
categories:
  - 学习
toc: false
date: 2019-02-08 21:28:28
---

1.React中
使用import react-css-modules时，一定要把`className`换成`styleName`（这个花了我一个半小时才解决）

2.同样是使用react-css-modules时，如果要绑定多个styleName，需要在export时的CSSModule那里加载第三个option参数：`{"allowMultiple":true}`，方可绑定多个styleName

3.React 动态绑定背景图片： ```style={\{\backgroundImage:`url(${i.img})`}\}\```

4.使用`React.Fragment`标签包裹众多标签可以使render时不会出现多余的没用的影响结构的div标签



5.Create-React-App __Deploy__ 时

如果想自动发布到Github的gh-pages，那么使用以下命令：
`npm install gh-pages --save-dev`


如果首次deploy中途不幸中断，想要再次deploy失败现实already exist时，使用以下命令清楚缓存：
`rm -rf node_modules/gh-pages/.cache`


6.BrowserRouter 在deploy到github page上时会有问题，需要切换成hashRouter
然后在config文件中设置homepage属性，要设置为部署后的域名，如：`"homepage": "https://sy2729.github.io/laodianwei"`


7.React中阻止冒泡
React中对事件进行了二次封装，如果是在React自身组件上监听的事件，需要用`e.nativeEvent.stopImmediatePropagation()`；

但如果是监听在document上的事件是无法阻止冒泡的，建议修改为对window的监听。