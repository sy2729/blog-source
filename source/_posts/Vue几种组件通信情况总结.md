---
title: Vue几种组件通信情况总结
date: 2018-09-13 02:19:21
tags: Vue 组件化
categories: 学习
---

在此总结一下Vue中常见组件通信的几种方法：

1.父子组件通信：

数据从上往下传递：
父组件通过v-bind的形式将参数传给子组件，子组件通过props接受父组件数据。
```
    <div id="app">
        <child :data = parentData></child>
    </div>

    <script>
        let child = {
            template: '<div>{{data}}</div>',
            props: ['data']
        };

        let app = new Vue({
            el: '#app',
            data: {
                parentData: '这是父组件即将传给子组件的数据'
            },

            components: {
                'child': child,
            },
        })

    </script>

    //在以上例子中，父组件的数据通过":"的形式以data这个变量将parentData数据传给了child组件，child用props语法接受这个变量并运用到自己的组件内容中。
```

数据从下往上传递：
```
    <div id="app">
        <child @child-event="dealChildEvent"></child>
        <p>Result:</p>
        <p>{{number}}</p>
    </div>

    <script>
        let child = {
            template: `<button @click="$emit('child-event')">加1</button>`,
            
        };

        let app = new Vue({
            el: '#app',
            data: {
                number: 0,
            },
            methods: {
                dealChildEvent(){
                    this.number++;
                }
            },

            components: {
                'child': child,
            },
        })

    </script>

    //在以上的例子中子组件button点击触发了我们自定义的事件'child-event'（注意自定义事件不推荐camelCase写法），然后在父组件中在这个子组件的壳上用‘@’语法对这个自定义事件进行了监听，再用父组件中的方法“dealChildEvent”对事件进行反馈和处理。
```



2.爷孙组件通信
Vue中无法隔代直接传数据，需要通过一代传一代的父子传递关系来接替。
数据从上往下传递：
```
    <div id="app">
        <child :data = parentData></child>
    </div>

    <script>   
        let childChild = {
            template: '<div>{{data}}</div>',
            props: ['data']
        };

        let child = {
            template: '<child-child :data = data></child-child>',
            props: ['data'],
            components: {
                'child-child': childChild,
            },
        };

        let app = new Vue({
            el: '#app',
            data: {
                parentData: '这是父组件即将传给子子组件的数据'
            },

            components: {
                'child': child,
            },
        });

    </script>
```


数据从下往下传递：
```
    <div id="app">
        <child @child-child-event=dealChildEvent></child>
        {{number}}
    </div>

    <script>   
        let childChild = {
            template: `<button @click="$emit('child-child-event')">Add 1</button>`,
        };

        let child = {
            template: `<child-child @child-child-event="$emit('child-child-event')"></child-child>`,
            props: ['data'],
            components: {
                'child-child': childChild,
            },
        };

        let app = new Vue({
            el: '#app',
            data: {
                number: 0,
            },
            methods: {
               dealChildEvent(){
                   this.number++;
               } 
            },

            components: {
                'child': child,
            },
        });

    </script>
```

3.兄弟组件通信
```
    <div id="app">
        <sibling1 :number = number @emit-from-sibling='add'></sibling1>
        <sibling1 :number = number @emit-from-sibling='add'></sibling1>
    </div>

    <script>   
        let sibling1 = {
            template: `
            <div>
                <div>{{number}}</div>
                <button @click="$emit('emit-from-sibling')">add 1</button>
            </div>
            `,
            props: ['number']
        };

        let sibling2 = {
            template: `
            <div>
                <div>{{number}}</div>
                <button @click="$emit('emit-from-sibling')">add 1</button>
            </div>
            `,
            props: ['number']
        };

        let app = new Vue({
            el: '#app',
            data: {
                number: 0, //这是父组件传给两个子组件的数据
            },
            methods: {
                add(){
                    this.number++;
                }
            },

            components: {
                sibling1,
                sibling2,                
            },
        });

    </script>
```