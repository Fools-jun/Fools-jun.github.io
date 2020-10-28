---
title: Vue之组件化开发
date: 2020-10-13 11:25:10
copyright: true
categories:
- 笔记
- 前端
tags:
- Vue
---



![](https://res.cloudinary.com/junpzx/image/upload/v1600667958/Vue相关/Vue入门/logo_j4jkbw.png)



<!-- less -->

# 什么是组件化？

将一个页面拆分成一个一个小的功能块，每个功能块完成属于自己这部分独立的功能

## Vue组件化思想

- 组件化是Vue.js中的重要思想

    - 它提供了一种抽象，让我们可以开发出一个个独立可复用的小组件来构造我们的应用。

    - 任何应用都会被抽象成一颗组件树

        ![Vue组件化思想](https://gitee.com/junpzx/blog-img/raw/master//img/20201013141918.png)

- 组件化思想的应用：
    - 有了组件化的思想，我们在之后的开发中就要充分的利用它。
    - 尽可能的将页面拆分成一个个小的、可复用的组件。
    - 这样让我们的代码更加方便组织和管理，并且扩展性也更强



# 注册组件的基本步骤

## 注册步骤

- 组件的使用分成三个步骤
    1. 创建组件构造器（调用Vue.extend()方法**创建组件构造器**）
        - 调用Vue.extend()创建的是一个组件构造器。
        - 通常在创建组件构造器时，传入template代表我们自定义组件的模板。
        - 该模板就是在使用到组件的地方，要显示的HTML代码。
    2. 注册组件（调用Vue.component()方法**注册组件**）
        - 调用Vue.component()是将刚才的组件构造器注册为一个组件，并且给它起一个组件的标签名称
        - 所以需要传递两个参数：
            1. 注册组件的标签吗
            2. 组件构造器
    3. 使用组件（在Vue实例的作用范围内**使用组件**）
        - 该组件必须在Vue挂载的实例中



## 注册代码

### 1.创建组件构造器

```javascript
const cpnC = Vue.extend({
        template:
            `<div>
                <h2>Hello</h2>
            </div>`
    });
```



### 2.注册组件

```javascript
Vue.component('c',cpnC)
```



### 3.使用组件

```html
<div id="app">
    <c></c>   <!-- 生效 -->
</div>
<c></c>  <!-- 不生效，因为该组件不在所挂载的Vue中 -->
```



# 组件的语法糖注册方式

```javascript
    // 语法糖注册全局组件方式
    Vue.component('c',{
        template:
            `<div>
                <h2>Hello</h2>
            </div>`
    })
    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue'
        }
    })
    
// ---------------------------------------------------------- 
    
    // 语法糖注册局部组件方式
    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue'
        },
        components:{
            'c':{
        		template:
                    `<div>
                        <h2>Hello</h2>
                    </div>`
    			}
        }
    })
```



# 全局组件和局部组件

- 调用Vue.compontent()注册组件时，组件的注册是全局的
    - 这意味着该组件可以在任意Vue的实例下使用。

```html
<body>
<div id="app">
    <c></c>
</div>

<script src="../../js/vue.js"></script>
<script>
    const cpnC = Vue.extend({
        template:
            `<div>
                <h2>Hello</h2>
            </div>`
    });
    Vue.component('c',cpnC)
    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue'
        }
    })

</script>
</body>
```



- 如果我们注册的组件时挂载在某个实例中，那么就是一个局部组件

``` HTML
<body>
<div id="app">
    <c></c>
</div>

<script src="../../js/vue.js"></script>
<script>
    const cpnC = Vue.extend({
        template:
            `<div>
                <h2>Hello</h2>
            </div>`
    });

    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue'
        },
        components:{
            c:cpnC
        }
    })

</script>
</body>
```







# 父组件和子组件

- 组件和组件之间存在层级关系
- 其中一种非常重要的关系就是父子组件的关系
- 当A组件中注册了组件B时，那么我们就可以看做A组件是B组件的父组件



## 父子组件的通信

- 通过props向子组件传递数据
- 通过事件向父组件发送消息

![父子组件通信的方式](https://gitee.com/junpzx/blog-img/raw/master//img/20201015153421.png)

### 父传子（props基本用法）

- 在组件中，使用选项props来声明需要从父级接收到的数据。
- props的值有两种方式：
    - 方式一：字符串数组，数组中的字符串就是传递时的名称。
    - 方式二：对象，对象可以设置传递时的类型，也可以设置默认值等。



#### 方式一：字符串数组

```html
<div id="app">
    <movies :movies="movies" :people="people"></movies>
</div>

<!--子组件template-->
<template id="moviesCpn">
    <div>
        <ul>
            <li v-for="item in movies">{{item}}</li>
        </ul>
        <ul>
            <li v-for="item in people">{{item}}</li>
        </ul>
    </div>
</template>
```

```javascript
<script src="../../js/vue.js"></script>
<script>
    // 创建子组件
    const moviesCpn = {
        template: '#moviesCpn',
        props: ['movies', 'people'],
        data() {
            return {}
        },
        methods: {}
    }


    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue',
            movies: ['肖申克救赎', '桃花侠大战菊花怪', '逐梦演艺圈'],
            people: ['张三', '李四']
        },
        components: {
            'movies': moviesCpn
        }
    })

</script>
```



#### 方式二：对象

当需要对props**进行类型等验证时**，就需要对象写法

- 验证支持哪些数据类型：
    - String
    - Number
    - Boolean
    - Array
    - Object
    - Date
    - Function
    - Symbol
- 当我们有定义构造函数时，验证也支持自定义的类型。

```javascript
<script src="../../js/vue.js"></script>
<script>
    // 创建子组件
    const moviesCpn = {
        template: '#moviesCpn',
        // props: ['movies', 'people'],
       props:{
            // 限制类型，默认值，是否必须
            movies:{
                type: Array,
                default:['电影1','电影2'],
                required:false
            },
            // 只限制类型
            people: Array
        },
        data() {
            return {}
        },
        methods: {}
    }


    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue',
            movies: ['肖申克救赎', '桃花侠大战菊花怪', '逐梦演艺圈'],
            people: ['张三', '李四']
        },
        components: {
            'movies': moviesCpn
        }
    })

</script>
```

**注意**：在以上代码的默认值写法中，如果vue的版本在2.5.x以下时，是不会报错的，但是如果vue版本在其之上，改写法会报以下错误：

```
Invalid default value for prop "movies": Props with type Object/Array must use a factory function to return the default value.
```

需要将其改为：

```javascript
// 创建子组件
    const moviesCpn = {
        template: '#moviesCpn',
        // props: ['movies', 'people'],
        props:{
            // 限制类型，默认值，是否必须
            movies:{
                type: Array,
                default(){
                  return ['电影1','电影2']
                },
                required:false
            },
            // 只限制类型
            people: Array
        },
        data() {
            return {}
        },
        methods: {}
    }
    ...
```

完整写法：

```javascript
	...
    // 创建子组件
    const moviesCpn = {
        template: '#moviesCpn',
        // props: ['movies', 'people'],
        props:{
            movies:{
                // 规定类型
                type: [Array,String],
                // 默认值
                default(){
                  return ['电影1','电影2']
                },
                //是否必须
                required:false,
                // 自定义验证
                validator(){

                }
            },
            // 只限制类型
            people: Array
        },
        data() {
            return {}
        },
        methods: {}
    }
	...
```

**注意：**如果props中的参数为驼峰命名的话，那么在父组件使用子组件传递参数时，需要将大写字母改成小写，并在其之前加上`-`

例如:

```javascript
	...
    // 创建子组件
    const moviesCpn = {
        template: '#moviesCpn',
        props:{
           myMessage: String
        },
    }
	...

```

那么在父组件传递参数时：

```html
<div id="app">
    <movies :my-message="message"></movies>
</div>
```



### 子传父（自定义事件）

- 什么时候需要自定义事件？
    - 当子组件需要向父组件传递数据时，就需要用到自定义事件了。
- 自定义事件的流程：
    - 在子组件中，通过$emit()来触发事件。
    - 在父组件中，通过v-on来监听子组件事件



```html
<div id="app">
    <movies-items-cpn :movie-items="movieItems" @movie-items-click="childBtnClick"></movies-items-cpn>
</div>

<!--子组件template-->
<template id="movies-sidebar">
    <div>
        <button v-for="movieItem in movieItems" @click="btnClick(movieItem)">
            {{movieItem.name}}
        </button>
    </div>
</template>
```



```javascript
<script src="../../js/vue.js"></script>
<script>
    // 创建子组件
    const moviesItemsCpn = {
        template: '#movies-sidebar',
        // props: ['movies', 'people'],
        props: {
            movieItems: Array
        },
        methods: {
            btnClick(item) {
                console.log("子组件中的点击事件")
                this.$emit('movie-items-click', item)
            }

        }

    }

    const app = new Vue({
        el: '#app',
        data: {
            movieItems: [
                {'id': '10001', 'name': '喜剧'},
                {'id': '10002', 'name': '科幻'},
                {'id': '10003', 'name': '悬疑'},
                {'id': '10004', 'name': '动作'}
            ]
        },
        components: {
            'moviesItemsCpn': moviesItemsCpn
        },
        methods: {
            childBtnClick(item) {
                console.log("父组件中的监听子组件的点击事件", item)
            }

        }
    })

</script>
```



## 父子组件的访问

