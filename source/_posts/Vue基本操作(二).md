---
title: Vue基本操作(二)
date: 2020-09-28 17:36:38
categories:
- 笔记
- 前端
tags:
- Vue
---



# 条件判断

## v-if

**使用:**

- v-if
- v-else-if
- v-else

```javascript
<div id="app" >
    <h2 v-if="display">{{message}}</h2>
    <h2 v-else>{{message + '2'}}</h2>
    <button @click="checkout()">切换</button>
</div>


<script src="../../js/vue.js"></script>
<script>
const app = new Vue({
    el: '#app',
    data: {
        message: 'Hello,Vue',
        display: true
    },
    methods:{
        checkout(){
            this.display = !this.display;
        }

    }
})

</script>
```



**注意:如果需要判断的层级很多,那么推荐使用计算属性**





## v-show

### 作用

主要用于控制某个元素是否显示



### 跟v-if的区别

- v-if:当条件为false时,包含v-if指令的元素,根本就不会存在在dom中
- v-show: 当条件为false时,包含v-show指令的元素添加了一个行内样式:display: none



### 总结

v-if是在控制是否在dom中显示,v-show是在设置行内样式display

如果需要频繁切换显示隐藏,那么推荐使用v-show

如果只是一次切换,那么推荐使用v-if





# 循环遍历

## 遍历数组

### 不需要下标

```javascript
<div id="app">
    <ul>
        <li v-for="item in names">{{item}}</li>
    </ul>
</div>
<script src="../../js/vue.js"></script>
<script>
const app = new Vue({
    el: '#app',
    data: {
        names: ['张三','李四','王五']
    }
})
</script>
```



### 需要下标

```javascript
<div id="app">
    <ul>
        <li v-for="(item,index) in names">
            {{index + 1}}.{{item}}
        </li>
    </ul>
</div>
<script src="../../js/vue.js"></script>
<script>
const app = new Vue({
    el: '#app',
    data: {
        names: ['张三','李四','王五']
    }
})
</script>
```





## 遍历对象

### 只要Value

```javascript
<div id="app">
    <ul>
        <li v-for="item in peopleInfo">{{item}}</li>
    </ul>
</div>
<script src="../../js/vue.js"></script>
<script>
const app = new Vue({
    el: '#app',
    data: {
        peopleInfo:{
            name: 'JunPzx',
            age: 18,
            height: 1.80
        }
    }
})
</script>
```



### Key和Value都要

```javascript
<div id="app">
    <ul>
        <li v-for="(item,key) in peopleInfo">{{key}}:{{item}}</li>
    </ul>
</div>
<script src="../../js/vue.js"></script>
<script>
const app = new Vue({
    el: '#app',
    data: {
        peopleInfo:{
            name: 'JunPzx',
            age: 18,
            height: 1.80
        }
    }
})
</script>
```



### Key和Vue和Index都要

```javascript
<div id="app">
    <ul>
        <li v-for="(item,key,index) in peopleInfo">{{index+1}}.{{key}}:{{item}}</li>
    </ul>
</div>
<script src="../../js/vue.js"></script>
<script>
const app = new Vue({
    el: '#app',
    data: {
        peopleInfo:{
            name: 'JunPzx',
            age: 18,
            height: 1.80
        }
    }
})
</script>
```



## 组件的Key属性

- 官方推荐我们在使用v-for时,给对应的元素或组件添加上一个:key属性

- 为什么需要这个key属性呢?

    - 这个其实和Vue的虚拟DOM的Diff算法有关系

- 当某一层有很多相同的节点时,也就是列表节点时,我们希望插入一个新的节点

    - 我们希望在B和C的中间添加一个F,Diff算法默认执行起来是这样的
    - 就是把C更新成F,D更新成C,E更新成D,最后在插入一个F,如下图左下

- 所以我们需要使用key来给每一个节点做一个唯一标识

    - Diff算法就可以正确的识别此节点
    - 找到正确的位置去插入新的节点

- **总结：key的作用主要就是为了高效的更新虚拟DOM**

    

    

    

    

![2020-09-29_14-40-59](https://gitee.com/junpzx/blog-img/raw/master//img/20200929144135.png)



**尽量保证key的唯一性**



