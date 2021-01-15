---
title: ES语法(持续更新)
categories:
  - 笔记
  - 前端
tags:
  - ECMAScript
copyright: true
abbrlink: 28374
date: 2020-09-24 18:00:30
---

记录学习或者使用过程中，ES6较之前新增的语法和某些不同的语法



<!--less -->

# let和var

事实上var的设计可以看成JavaScript语言设计上的错误,但是这种错误多半不能修复和移除,因为需要向后兼容.

- 大概十年前,Brendan Eich就决定修复这个问题,于是他添加了一个新的关键字:let
- 我们可以将let看成更加完美的var



## var问题分析

### **代码**

```javascript
<body>
    <button>1</button>
    <button>2</button>
    <button>3</button>
    <button>4</button>
    <button>5</button>

<script>
    var btns = document.getElementsByTagName("button");
    for (var i=0;i<btns.length;i++){
      btns[i].addEventListener('click',function () {
        console.log('第'+i+'个按钮被点击')
      })
    }
</script>
```



### **运行结果**

![运行结果](https://gitee.com/junpzx/blog-img/raw/master/img/20200925103505.png)

### 问题

#### **为什么点击每一个按钮的打印结果都为** `第五个按钮被点击`**?**

​	这是因为当给btns[i]添加click事件时,其事件内部使用该变量i,当循环继续执行,事件内部引用的i的值被修改了,所以才会在打印时全部都是第五个按钮被点击

```javascript
<script>
    var btns = document.getElementsByTagName("button");
    for (var i=0;i<btns.length;i++){
      btns[i].addEventListener('click',function () {
        console.log('第'+i+'个按钮被点击')
      })
    }
    i=3;
</script>
```

如果像上面那样,在事件添加完毕后,将i的值修改为3,那么所有按钮的打印结果都为`第3个按钮被点击`



### 解决方案

#### 方案一:使用闭包

##### 代码

```javascript
<script>
    var btns = document.getElementsByTagName("button");
    for (var i=0;i<btns.length;i++){
      (function (num) {
        btns[num].addEventListener('click',function () {
          console.log('第'+num+'个按钮被点击')
        })
      })(i)
    }
    i=3;
</script>
```

##### 运行结果

![运行结果](https://gitee.com/junpzx/blog-img/raw/master/img/20200925105049.png)

##### 分析

**为什么能使用闭包来解决该问题?**

​	这个就类似于Java中的值传递和引用传递,当你不使用闭包时,你所有按钮事件内部的i是同一个i,也就是它们的i的引用地址是相同的,那么你修改了i的值,因为你引用的都是同一个i,那么得到那个结果也就不足为奇了。

​	如果使用了闭包,就类似于,你给每一个按钮添加事件时,将i传入进去,函数内部使用局部变量num用来保存i的值(**函数是一个作用域**),那么我使用的就是num的值,而不是i的引用,所以你修改了i的值,不关num的事,所以最后的输出结果为所期待的结果



#### 方案二:使用let

##### 代码

```javascript
<script>
  const btns = document.getElementsByTagName("button");
  for (let i = 0; i < btns.length; i++) {
    btns[i].addEventListener('click', function () {
      console.log('第' + i + '个按钮被点击')
    })
  }
  i = 3;
</script>
```

##### 运行结果

![运行结果](https://gitee.com/junpzx/blog-img/raw/master/img/20200925111537.png)

##### 分析

ES6中加入了let,let它是有if和for的块级作用域。



## 结论

- ES5之前因为if和for都没有块级作用域的概念,所以在很多使用,我们必须借助于function的作用域来解决应用外面变量的问题。
- ES6中加入了let,let它是有if和for的块级作用域。





# const

## 介绍

- 在很多语言中都已经存在,主要的作用是将某个变量修饰成常量。
- 在JavaScript中也是如此,使用const修饰的标识符为常量,不可再次赋值。



## 使用场景

- 当我们修饰的标识符不会被再次赋值时,就可以使用const来保证数据的安全性。



## 注意

1. 被赋值的常量不可被再次赋值
2. 在初始化常量时必须赋值
3. 常量定义的对象不能修改,指的是该对象的引用不能修改,对象内的属性值是可以修改的



# 对象字面量的增强写法

**什么叫做字面量?**

```
例如:
普通创建对象是这样创建的: const obj = new Object()
字面量就是: const obj = {}
这个大括号就叫做字面量
```



## 属性的增强写法

```javascript
const age = 18
const name = '张三'
const height = 1.88

//ES5写法
const people = {
    age : age,
    name : name,
    height : height
}

//ES6写法
const people = {
    age,
    name,
    height
}

//以上两种写法最后的结果是等价的
```



## 函数的增强写法

```javascript
//ES5写法
const people = {
    eat: function(){
        //方法体
    }
}

//ES6写法
const people = {
    eat(){
		//方法体        
    }
}

//以上两种写法最后的结果是等价的
```





# 循环语法

## 普通for循环

```javascript
for (let i = 0; i < array.length; i++) {
     console.log(array[i])
}
```



## 使用for  in 循环

```javascript
for(let i in array){
   console.log(array[i])
}
```



## 使用for of循环

```javascript
for (let value of array){
     console.log(value)
}
```



# 定义字符串

- 使用单引号‘ ’
- 使用\`\`,相较于‘ ’而言，``可以直接换行，而不用加+号