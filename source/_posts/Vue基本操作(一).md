---
title: Vue基本操作(一)
categories:
  - 笔记
  - 前端
tags:
  - Vue
copyright: true
abbrlink: 18110
date: 2020-09-21 17:01:41
---

插值操作的介绍和使用

绑定属性的介绍和使用

计算属性的介绍和使用

事件监听的介绍和使用

表单输入绑定（v-model）的介绍和使用

![](https://res.cloudinary.com/junpzx/image/upload/v1600667958/Vue相关/Vue入门/logo_j4jkbw.png)

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

![运行结果](https://res.cloudinary.com/junpzx/image/upload/v1600757346/Vue相关/Vue的基本操作/1_uq1zcr.png)





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

![运行结果](https://res.cloudinary.com/junpzx/image/upload/v1600757514/Vue相关/Vue的基本操作/2_tjrvbj.png)





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

![运行结果](https://res.cloudinary.com/junpzx/image/upload/v1600757974/Vue相关/Vue的基本操作/3_zn2do2.png)





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

![运行结果](https://res.cloudinary.com/junpzx/image/upload/v1600758176/Vue相关/Vue的基本操作/4_rortqw.png)



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

![运行结果](https://res.cloudinary.com/junpzx/image/upload/v1600758367/Vue相关/Vue的基本操作/5_xns1np.png)



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

![运行结果](https://res.cloudinary.com/junpzx/image/upload/v1600760783/Vue相关/Vue的基本操作/6_vfzcbn.png)





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



![运行结果](https://gitee.com/junpzx/blog-img/raw/master//img/20200924141740.png)





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

![运行结果](https://gitee.com/junpzx/blog-img/raw/master//img/20200924141657.png)



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



![运行结果](https://gitee.com/junpzx/blog-img/raw/master//img/20200924163151.png)





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

![运行结果](https://gitee.com/junpzx/blog-img/raw/master//img/20200924164456.png)







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

![运行结果](https://gitee.com/junpzx/blog-img/raw/master//img/20200928163634.png)



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





# 表单输入绑定

## v-model介绍

你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

> **注意**：`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。



`v-model` 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 `value` 属性和 `input` 事件；
- checkbox 和 radio 使用 `checked` 属性和 `change` 事件；
- select 字段将 `value` 作为 prop 并将 `change` 作为事件。

> **注意**：对于需要使用[输入法](https://zh.wikipedia.org/wiki/输入法) (如中文、日文、韩文等) 的语言，你会发现 `v-model` 不会在输入法组合文字过程中得到更新。如果你也想处理这个过程，请使用 `input` 事件。



## v-model基本使用

### 文本（input）

```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```



### 多行文本（textarea）

```html
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```

> **注意**：在文本区域插值 (`<textarea>{{text}}</textarea>`) 并不会生效，应用 `v-model` 来代替。



### 复选框（checkbox）

#### 单个复选框绑定到boolean值：

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```



#### 多个复选框，绑定到同一个数组：

```html
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Checked names: {{ checkedNames }}</span>
```

```javascript
new Vue({
  el: '...',
  data: {
    checkedNames: []
  }
})
```



### 单选按钮（radio）

```html
<div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
```

```javascript
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
```



### 选择框（select）

#### 单选

````html
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
````

```javascript
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```

> **注意**：如果 `v-model` 表达式的初始值未能匹配任何选项，`<select>` 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。



#### 多选时 (绑定到一个数组)：

```html
<div id="example-6">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
```

```javascript
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
```



用 `v-for` 渲染的动态选项：

```html
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
```

```javascript
new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```



## 值绑定

对于单选按钮，复选框及选择框的选项，`v-model` 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：

```html
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle">

<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

但是有时我们可能想把值绑定到 Vue 实例的一个动态 property 上，这时可以用 `v-bind` 实现，并且这个 property 的值可以不是字符串。



### 复选框

```html
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>
```

```javascript
// 当选中时
vm.toggle === 'yes'
// 当没有选中时
vm.toggle === 'no'
```

> **注意**：这里的 `true-value` 和 `false-value` attribute 并不会影响输入控件的 `value` attribute，因为浏览器在提交表单时并不会包含未被选中的复选框。如果要确保表单中这两个值中的一个能够被提交，(即“yes”或“no”)，请换用单选按钮。



### 单选按钮

```html
<input type="radio" v-model="pick" v-bind:value="a">
```

```javascript
// 当选中时
vm.pick === vm.a
```



### 选择框的选项

```html
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```

```javascript
// 当选中时
typeof vm.selected // => 'object'
vm.selected.number // => 123
```



## 修饰符

### `.lazy`

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步：

```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```



### `.number`

如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：

```html
<input v-model.number="age" type="number">
```

这通常很有用，因为即使在 `type="number"` 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。



### `.trim`

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

```html
<input v-model.trim="msg">
```



