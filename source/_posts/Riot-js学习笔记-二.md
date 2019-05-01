---
title: Riot-js学习笔记(二）
date: 2018-02-08 09:21:52
tags: Riot.js
categories: 学习
---

### Riot中的生命周期
Riot中通过几个生命周期勾子来实现组件的装卸和更新。了解整个组件从加载到更新再到卸载的过程有利于帮助我们理解如何利用周期实现一些功能以及避免一些坑。

Riot中的生命周期分别有`before-mount`, `mount`, `update`, `updated`, `before-unmount`, `unmount`几个，但是最关键也是最常为我们所用的是`mount`, `update`, `unmount`几个。

在触发用户事件之后，Riot会自动在当前组件运行`this.update()`以更新组件，更新所有当前组件中有变化的值来渲染整个页面

通常情况下是不需要去关注生命周期的更迭的，因为Riot已经自动做好了这些事。但是有一种情况会需要手动添加`update`。

JS是单线程运行机制，在遇到向服务器请求资源时，采用Asynchronous的异步运行机制，JS会自动跳过这一段运行之后的代码。此时，请求的资源和新数据还没有得到，而Riot已经进行了更新。所以之后获取的新数据并没有渲染到视图层上，只是更新了代码中变量的值，所以这个时候需要手动`update`

可以通过SetTimeout的方式模拟异步加载，来实验Riot中这种需要手动update的方式。
```
<main>

  <p>This is the data to be updated { data }</p>

  <button onclick = { change }>Change data</button>

<script>

  var that = this;

  this.data = 0;
  this.change = function() {
    SetTimeout(function() {
        that.data++;

        //没有以下的手动update，点击button后p中的data值并不会改变；
        that.update();

      }, 3000)
  }


</script>  

</main>
```
以上代码中使用新建变量that替代this的原因是`SetTimeout`中的上下文环境已经改变了，this不再指向当前的tag，所以先用变量保存指向当前tag环境的this的指向，然后再在`SetTimeout`中使用。


### Riot中的DOM操作
Riot中我们不需要再使用`document.querySelector`来选取元素。Riot提供了更简洁的解决方案——`ref`；

```
<main>

  <input type = "text"  ref="getInput" onkeypress = { change }><input>
  <p>This is the text from the input: { textValue }</p>

  <script>
    this.textValue = "initial";
    this.change = function() {
      this.textValue = this.refs.getInput.value;
    }
  </script>

</main>
```
以上几行代码即实现了输入框和展示栏的动态绑定。



### Riot中的父子组件通信
父子组件通信通常有几种情况：
* 父组件传递数据给子组件，子组件接收数据；
* 父组件主动获取子组件中的数据；
* 子组件主动获取父组件中的数据；

#### 第一种情况通过使用Riot中的`opts`可以实现，以下是父组件：
```
<parent>

  <child title = "这是我要传给子组件的消息"></child>

  <script>

  </script>

</parent>
```

以下是子组件：

```
<child>

  <p>{ this.opts.title}</p>

  <script>

  </script>

</child>
```
如此，父组件的信息就被传递到了子组件。`title`只是自定义的一个attribute，可以自定义任何便于理解的名字，而`title`的赋值除了指定赋值以外，也可以直接关联到父级组件的数据，比如`title = {this.XXX}`，如此以来，opts的功能便十分强大和灵活。


#### 第二种情况通过使用Riot中的`tags`关键词实现，以下是父组件：
```
<parent>

  <p>{ childData }</p>

  <child></child>

  <script>
    this.childData = this.tags.
  </script>

</parent>
```

以下是子组件：

```
<child>

  <p>{ this.data }</p>

  <script>
    this.data = "我要等着父级组件来取我"
  </script>

</child>
```
