---
title: Vue入门
categories:
  - 笔记
  - 前端
tags:
  - Vue
copyright: true
abbrlink: 58210
date: 2020-09-21 13:20:27
---

简单介绍一下vue是什么,和vue的几种安装方式,vue的简单使用 

![](https://res.cloudinary.com/junpzx/image/upload/v1600667958/Vue相关/Vue入门/logo_j4jkbw.png)

<!-- less -->

# 介绍

## Vue.js是什么?

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。



## Vue是一个渐进式框架,什么是渐进式的呢?

- 渐进式意味着你可以将Vue作为你应用的一部分嵌入其中,带来更丰富的交互体验
- 或者如果你希望将更多的业务逻辑使用Vue实现,那么Vue的核心库以及其生态系统
- 比如Code+Vue-router+Vuex,也可以满足你各种各样的需求



## Vue有很多特点和Web开发中常用的高级功能:

- 解耦视图和数据
- 可复用组件
- 前端路由技术
- 状态管理
- 虚拟DOM



# 安装

- 直接CDN导入(CDN会选择距离最近的服务器进行下载,会提高下载效率)

  ```js
  <!-- 开发环境版本,包含了有帮助的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <!-- 生产环境版本,优化了尺寸和速度 -->
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  ```

- 下载和引入

  开发环境   https://vue.js.org/js/vue.js

  生产环境   https://vuejs.org/js/vue.min.js

- npm安装

  ```
  # 最新稳定版
  $ npm install vue
  ```

# Vue的基本使用

## 案例一:Hello Vue

```javascript
// let用于修饰变量
let name = "vue"
// const用于修饰常量
const age = "vue"
```

```javascript
<body>
<div id="app">{{message}}</div>
<div>{{message}}</div>
<script src="js/vue.js"></script>
<script>
    // 编程范式: 声明式编程
    const app = new Vue({
        el: '#app', //用于挂载要管理的元素
        data: { // 定义数据
            message: '你好呀,pzx!'
        }
    })

    //使用js的做法(编程范式: 命令式编程)
    //1.创建div元素,设置id属性

    //2.定义变量叫message

    //3.将message变量放在前面的div元素中显示

    //4.修改message的值

    //5.将message的值重新赋值给div
</script>
</body>
```

**以上代码的执行结果为:**

![运行结果](https://res.cloudinary.com/junpzx/image/upload/v1600669908/Vue相关/Vue入门/1_wsql5b.png)

**代码做了什么事情:**

1. 首先创建了一个Vue对象
2. 创建Vue对象的时候,传入了一些options:{}
   - {}中包含了el属性:该属性决定了这个Vue对象挂载到哪一个元素上
   - {}中包含了data属性:该属性通常会存储一些数据
     - 这些数据可以是我们直接定义出来的,比如像上面那样
     - 也可能是来自网络,从服务器加载出来的



## 案例二:Vue的列表显示

```javascript
<body>
<div id="app">
    <ul>
        <li v-for="item in tags">{{item}}</li>
    </ul>
</div>

<script src="js/vue.js"></script>
<script>
    const app = new Vue({
        el: '#app',
        data:{
            tags: ['Java','JavaScript','C','C++']
        }
    })


</script>
</body>
```

**以上代码的执行结果为:**

![运行结果](https://res.cloudinary.com/junpzx/image/upload/v1600671118/Vue相关/Vue入门/2_b7haeu.png)

这个地方主要使用**v-for**指令,而且它还是响应式的

- 也就是当数组中的数据发生变化时,界面会自动改变



## 案例三:计数器

```javascript
<body>
<div id="app">
    <h2>当前计数:{{counter}}</h2>
    <!--    <button @click="counter++">+</button>-->
    <!--    <button v-on:click="counter-1 < 0 ? counter=0 : counter--;">-</button>-->
    <button v-on:click="add">+</button>
    <button v-on:click="subtraction">-</button>
</div>yi

<script src="js/vue.js"></script>
<script> 
    const app = new Vue({
        el: '#app',
        data: {
            counter: 0
        },
        methods: {
            add: function () {
                this.counter++;
                console.log("+已被点击")
            },

            subtraction: function () {
                this.counter-1 < 0?this.counter=0:this.counter--;
                console.log("-已被点击")

            }
        }
    })

</script>
</body>
```

**以上代码代码的执行结果为:**

![运行结果](https://res.cloudinary.com/junpzx/image/upload/v1600674786/Vue相关/Vue入门/3_dqgp6p.png)



**新的属性: methods,该属性用于在Vue对象中定义方法**

**新的指令: @click(等同于v-on:click,@click为前面的语法糖),该指令用于监听某个元素的点击事件,并且需要指定当发生点击时,执行的方法(方法通常是methods中定义的方法)**









