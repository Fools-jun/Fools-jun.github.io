---
title: Vue基本操作(一)
date: 2020-09-21 17:01:41
categories:
- 笔记
- 前端
tags:
- Vue
---

插值操作的介绍和使用

绑定属性的介绍和使用

计算属性的介绍和使用

事件监听的介绍和使用

![https://res.cloudinary.com/junpzx/image/upload/v1600667958/Vue%E7%9B%B8%E5%85%B3/Vue%E5%85%A5%E9%97%A8/logo_j4jkbw.png](https://res.cloudinary.com/junpzx/image/upload/v1600667958/Vue相关/Vue入门/logo_j4jkbw.png)

<!-- less -->

# 插值操作

## Mustache语法

使用双大括号,也就是mustache语法

**代码**

```javascript
<body>
<div id="app">
    <h2>{{message}}</h2>
    <h2>{{name}}</h2>
    <h2>{{age}}</h2>
    <h2>{{name + message}}</h2>
    <h2>{{name + ' ' + message }}</h2>
    <h2>{{name}}  {{message}}</h2>
    <h2>{{age * 2}}</h2>
</div>

<script src="../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好',
      name: '君',
      age: 17
    }
  })
</script>
```



**运行结果**

![https://res.cloudinary.com/junpzx/image/upload/v1600757346/Vue%E7%9B%B8%E5%85%B3/Vue%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C/1_uq1zcr.png](https://res.cloudinary.com/junpzx/image/upload/v1600757346/Vue相关/Vue的基本操作/1_uq1zcr.png)





## v-once指令

不期望某个地方的值是响应式的,就可以使用该指令

**代码**

```javascript
<body>
<div id="app">
    <h2>{{message}}</h2>
    <h2  v-once>{{message}}</h2>
</div>

<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
      el: '#app',
      data:{
        message:'Hello Vue'
      }
    })
</script>
</body>
```



**运行结果**

![https://res.cloudinary.com/junpzx/image/upload/v1600757514/Vue%E7%9B%B8%E5%85%B3/Vue%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C/2_tjrvbj.png](https://res.cloudinary.com/junpzx/image/upload/v1600757514/Vue相关/Vue的基本操作/2_tjrvbj.png)





## v-html指令

**代码**

```javascript
<body>
<div id="app">
    <h2>{{url}}</h2>
    <h2 v-html="url"></h2>
</div>

<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
      el: '#app',
      data:{
        message:'Hello Vue',
        url: '<a href="http://www.baidu.com">百度一下</a>'
      }
    })
</script>
</body>
```



**运行结果**

![https://res.cloudinary.com/junpzx/image/upload/v1600757974/Vue%E7%9B%B8%E5%85%B3/Vue%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C/3_zn2do2.png](https://res.cloudinary.com/junpzx/image/upload/v1600757974/Vue相关/Vue的基本操作/3_zn2do2.png)





## v-text指令

使用较少,因为不够灵活

**代码**

```javascript
<body>
<div id="app">
    <h2>{{message}}</h2>
    <h2 v-text="message"></h2>
</div>

<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
      el: '#app',
      data:{
        message:'Hello Vue'
      }
    })
</script>
</body>
```



**运行结果**

![https://res.cloudinary.com/junpzx/image/upload/v1600758176/Vue%E7%9B%B8%E5%85%B3/Vue%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C/4_rortqw.png](https://res.cloudinary.com/junpzx/image/upload/v1600758176/Vue相关/Vue的基本操作/4_rortqw.png)



## v-pre指令

**代码**

```javascript
<body>
<div id="app">
    <h2>{{message}}</h2>
    <h2 v-pre>{{message}}</h2>
</div>

<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
      el: '#app',
      data:{
        message:'Hello Vue'
      }
    })
</script>
</body>
```



**运行结果**

![https://res.cloudinary.com/junpzx/image/upload/v1600758367/Vue%E7%9B%B8%E5%85%B3/Vue%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C/5_xns1np.png](https://res.cloudinary.com/junpzx/image/upload/v1600758367/Vue相关/Vue的基本操作/5_xns1np.png)



## v-cloak指令

cloak:斗篷

作用:当vue对象未创建时,将会显示v-cloak属性,而当vue对象创建完成以后,属性将自动删除,那么如果vue对象还未创建,可能发生页面上显示这个结果

```
<h2>{{message}}<h2>
```

而我们并不期望显示这个结果,而是如果当时vue对象未创建,直接不显示就好



**代码**

```javascript
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        [v-cloak]{
            display: none;
        }
    </style>
</head>
<body>
<div id="app">
    <h2>{{message}}</h2>
    <h2 v-cloak>{{message}}</h2>
</div>

<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
      el: '#app',
      data:{
        message:'Hello Vue'
      }
    })
</script>
</body>
```

运行结果因为该代码的结果图不好表现,所以没有截图



# 绑定属性

## v-bind指令基本使用

**作用**

动态绑定属性

- 比如动态绑定a元素的href属性
- 比如动态绑定img元素的src属性

**缩写(语法糖)**

:

**预期**

any(with argument) | Object(without argument)

**参数**

attrOrProp(optional)



**代码**

```javascript
<body>
<div id="app">
    <img :src="imgUrl" alt="">
    <!--等同于<img v-bind:src="imgUrl" alt="">-->
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
      el: '#app',
      data: {
        imgUrl: 'https://img30.360buyimg.com/babel/s1180x940_jfs/t1/147101/20/8922/101180/5f685a77Eac4e4d8f/cbc61d2e465fde2c.jpg.webp'

      }
    })
</script>
</body>
```

![https://res.cloudinary.com/junpzx/image/upload/v1600760783/Vue%E7%9B%B8%E5%85%B3/Vue%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C/6_vfzcbn.png](https://res.cloudinary.com/junpzx/image/upload/v1600760783/Vue相关/Vue的基本操作/6_vfzcbn.png)





## v-bind动态绑定class

### 对象语法

#### 第一种方式

在:class="" 中直接使用对象

**代码**

```javascript
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .active{
            color: red;
        }
    </style> 
</head>
<body>
<div id="app">
    <h2 :class="{active:isActive}">{{message}}</h2>
    <button @click="isActive=!isActive">变变变</button>
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
      el: '#app',
      data: {
        message: "Hello Vue",
        active: 'active',
        isActive: true
      }
    })
</script>
</body>
```



#### 第二种方式

在:class=""中使用方法

**代码**

```javascript
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .active{
            color: red;
        }
    </style>
</head>
<body>
<div id="app">
    <h2 :class="{active:isActive}">{{message}}</h2>
    <h2 :class="getClass()">{{message}}</h2>
    <button @click="isActive=!isActive">变变变</button>
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
      el: '#app',
      data: {
        message: "Hello Vue",
        active: 'active',
        isActive: true
      },
      methods:{
        getClass:function (){
          return {active: this.isActive}
        }

      }
    })
</script>
</body>
```



### 数组语法

**代码**

```javascript
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .active{
            color: red;
        }
    </style>
</head>
<body>
<div id="app">
    <!--请注意,数组内部如果元素加'',会将其当做字符串解析,如果不加'',则会当作变量去查找解析-->
    <!--这样写,那么就不是动态的呢-->
    <h2 :class="['active','line']">{{message}}</h2>
    <button @click="isActive=!isActive">变变变</button>
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
      el: '#app',
      data: {
        message: "Hello Vue",
        active: 'active',
        isActive: true
      }
    })
</script>
```



## v-bind动态绑定style

多用于组件的在多个地方的自定义的动态设置组件的样式

### 对象语法

**代码**

```javascript
<div id="app">
    <h2>{{message}}</h2>
    <!-- <h2 :style="{key(属性名): value(属性值)}">{{message}}</h2>-->
    <!--如果属性值不加单引号,会将属性值当做变量-->
    <h2 :style="{fontSize: '50px'}">{{message}}</h2>
    <h2 :style="{fontSize: fontSizeZ}">{{message}}</h2>
    <h2 :style="{fontSize: fontSizeZ2 + 'px'}">{{message}}</h2>
    <h2 :style="{fontSize: fontSizeZ2 + 'px',color: colorZ}">{{message}}</h2>
    <h2 :style="getStyles()">{{message}}</h2>
</div>
<script src="../../js/vue.js"></script>
<script>
    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue',
            fontSizeZ: '50px',
            fontSizeZ2: 50,
            colorZ: 'red'
        },
        methods: {
            getStyles:function () {
                return {fontSize: this.fontSizeZ2 + 'px',color: this.colorZ}
            }
        }
    })
</script>
```



**运行结果**



![20200924111447](https://gitee.com/junpzx/blog-img/raw/master//img/20200924141740.png)





### 数组语法

**代码**

```javascript
<div id="app">
    <h2>{{message}}</h2>
    <h2 :style="[baseStyle]">{{message}}</h2>
</div>
<script src="../../js/vue.js"></script>
<script>
    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue',
            baseStyle: {
                color: 'red',
                fontSize: '50px'
            }
        }
    })
</script>
```



**运行结果**

![20200924111816](https://gitee.com/junpzx/blog-img/raw/master//img/20200924141657.png)



# 计算属性

在模板中绑定表达式是非常便利的，但是它们实际上只用于简单的操作。模板是为了描述视图的结构。在模板中放入太多的逻辑会让模板过重且难以维护。这就是为什么 Vue.js 将绑定表达式限制为一个表达式。如果需要多于一个表达式的逻辑，应当使用**计算属性**。



## 基础使用

### 案例一

拼接两个变量

**代码**

```javascript
<div id="app">
    <h2>{{message + ' ' + message2}}</h2>
    <h2>{{message}} {{message2}}</h2>
    <h2>{{getAllMessage()}}</h2>
    <h2>{{allMessage}}</h2>
</div>


<script src="../../js/vue.js"></script>
<script>
    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue',
            message2: 'Bye,Vue'
        },
        methods: {
            getAllMessage: function () {
                return this.message + ' ' + this.message2
            }
        },
        // 计算属性,给函数取名称时最好按照变量的命名方式
        computed: {
            allMessage: function () {
                return this.message + ' ' + this.message2;
            }
        }
    })

</script>
```



**运行结果**



![20200924163144](https://gitee.com/junpzx/blog-img/raw/master//img/20200924163151.png)





### 案例二

计算一个数组里面,所有书籍对象的总价格

**代码**

```javascript
<div id="app">
    <h2>总价格: {{totalPrice}}</h2>
</div>


<script src="../../js/vue.js"></script>
<script>
    const app = new Vue({
        el: '#app',
        data: {
            books: [
                {
                    id: '101',
                    name: 'Java',
                    price: 200
                },
                {
                    id: '102',
                    name: 'C',
                    price: 100
                },
                {
                    id: '103',
                    name: 'C++',
                    price: 400
                },
                {
                    id: '104',
                    name: 'Python',
                    price: 300
                }
            ]
        },
        computed: {
            totalPrice: function () {
                let total = 0;
                // 遍历的三种方式
                //第一种
                for (let i = 0; i < this.books.length; i++) {
                    total += this.books[i].price;
                }
                //第二种
               /* for (let i in this.books) {
                    total += this.books[i].price;
                }*/
                //第三种
               /* for (let book of this.books) {
                    total += book.price;
                }*/
                return total;
            }
        }
    })

</script>
```



**运行结果**

![20200924164447](https://gitee.com/junpzx/blog-img/raw/master//img/20200924164456.png)







## Getter和Setter

计算属性一般都是只读属性,所以只有get方法,因为只有get方法,所以可以简写

而这又解释了为什么在调用计算属性的时候不用加()

```javascript
const app = new Vue({
    el: '#app',
    data: {
        message: 'Hello,Vue',
        message2: 'Bye,Vue'
    },
    computed: {
        // 简写
        // allMessage: function () {
        //     return this.message+ ' ' + this.message2
        // }

        // 完整写法
        // 计算属性一般是没有set方法的,只读属性
         allMessage: {
             get: function (){
                return this.message + ' ' + this.message2;
             },
             set: function (newValue){
                 const messages = newValue.split(' ');
                 this.message = messages[0];
                 this.message2 = messages[1];
             }
         }
    }
})
```





## 计算属性和Methods的对比

- methods方法和computed计算属性，两种方式的最终结果确实是完全相同
- 计算属性是基于它们的响应式依赖进行缓存的。只在相关响应式依赖发生改变时它们才会重新求值，多次访问计算属性会立即返回之前的计算结果，而不必再次执行函数。
- methods方法，每当触发重新渲染时，调用方法将总会再次执行函数。
- 对于任何复杂逻辑，你都应当使用计算属性





# 事件监听

## v-on介绍

- 作用: 绑定事件监听器
- 缩写: @
- 预期: Function | Inline Statement | Object
- 参数: event



## 基本使用

```javascript
<div id="app">
    <h2>{{counter}}</h2>
    <button @click="counter++">+</button>
    <button @click="counter--">-</button>
    <button v-on:click="increment">+</button>
    <button v-on:click="decrement">-</button>
</div>


<script src="../../js/vue.js"></script>
<script>
const app = new Vue({
    el: '#app',
    data: {
        message: 'Hello,Vue',
        counter: 0
    },
    methods:{
        increment(){
            this.counter++;
        },
        decrement(){
            this.counter--;
        }

    }
})
</script>
```



## v-on参数

当通过methods中定义方法,以供@click调用时,需要**注意参数问题:**

- 情况一: 如果该方法不需要额外参数,那么方法后的()可以不添加。
    - 但是需要注意: 如果方法本身中有一个参数,那么会默认将原生事件event参数传递进去。
- 情况二: 如果需要同时传入某个参数,同时需要event时,可以通过$event传入事件。



**代码:**

```JavaScript
<div id="app">
    <!--事件调用的方法没有参数-->
    <button @click="btn1click">按钮1</button>
    <button @click="btn1click()">按钮1</button>
    <br/>
    -----------------------------------------------------
    <br/>
    <!--在事件定义时,写函数时省略了小括号,但是方法本身是需要一个参数的,这个时候vue
    会默认将浏览器产生的event事件对象作为参数传入到方法-->
    <button @click="btn2click('test')">按钮2</button>
    <button @click="btn2click()">按钮2</button>
    <button @click="btn2click">按钮2</button>
    <br/>
    ------------------------------------------------------
    <br/>
    <!--方法定义时,我们需要event对象,同时又需要其他参数-->
    <!--在调用方法时,如何手动的获取到浏览器产生的event对象: $event-->
    <button @click="btn3click">按钮3</button>
    <button @click="btn3click()">按钮3</button>
    <button @click="btn3click('test')">按钮3</button>
    <button @click="btn3click('test', $event)">按钮3</button>
    <br/>
    ------------------------------------------------------
</div>


<script src="../../js/vue.js"></script>
<script>
    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello,Vue'
        },
        methods: {
            btn1click() {
                console.log("btn1Click");
            },
            btn2click(event) {
                console.log("btn2Click" + "-----" + event);
            },
            btn3click(option,event){
                console.log("btn3wuClick" + "-----option:" + option+"------event:"+event);
            }
        }
    })

</script>
```

**运行结果:**

![2020-09-28_16-33-09](https://gitee.com/junpzx/blog-img/raw/master//img/20200928163634.png)



**注意:如果方法需要参数,传入的参数为String类型,那么需要加上``' ' ``,如果不加的话,vue会将其当做变量去data中进行查找,如果查找不到将会报错**



## v-on修饰符



- `.stop` - 调用 `event.stopPropagation()`。
- `.prevent` - 调用 `event.preventDefault()`。
- `.capture` - 添加事件侦听器时使用 capture 模式。
- `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
- `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
- `.native` - 监听组件根元素的原生事件。
- `.once` - 只触发一次回调。
- `.left` - (2.2.0) 只当点击鼠标左键时触发。
- `.right` - (2.2.0) 只当点击鼠标右键时触发。
- `.middle` - (2.2.0) 只当点击鼠标中键时触发。
- `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器



**示例:**

```html
<!-- 绑定一个 attribute -->
<img v-bind:src="imageSrc">

<!-- 动态 attribute 名 (2.6.0+) -->
<button v-bind:[key]="value"></button>

<!-- 缩写 -->
<img :src="imageSrc">

<!-- 动态 attribute 名缩写 (2.6.0+) -->
<button :[key]="value"></button>

<!-- 内联字符串拼接 -->
<img :src="'/path/to/images/' + fileName">

<!-- class 绑定 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">

<!-- style 绑定 -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>

<!-- 绑定一个全是 attribute 的对象 -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- 通过 prop 修饰符绑定 DOM attribute -->
<div v-bind:text-content.prop="text"></div>

<!-- prop 绑定。“prop”必须在 my-component 中声明。-->
<my-component :prop="someThing"></my-component>

<!-- 通过 $props 将父组件的 props 一起传给子组件 -->
<child-component v-bind="$props"></child-component>

<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>
```



## v-on事件

[点击查看](https://www.junpzx.cn/2020/09/28/JavaScript%E4%BA%8B%E4%BB%B6/#more)

