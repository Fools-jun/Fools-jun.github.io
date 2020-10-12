---
title: JavaScript高阶函数
date: 2020-09-30 10:18:48
categories:
- 笔记
- 前端
tags:
- JavaScript

---

什么是函数式编程？JavaScript高阶函数

<!-- less -->

# 函数式编程

> **函数式编程**（英语：**functional programming**）或称**函数程序设计**、**泛函编程**，是一种[编程范式](https://zh.wikipedia.org/wiki/编程范式)，它将[电脑运算](https://zh.wikipedia.org/wiki/電腦運算)视为[函数](https://zh.wikipedia.org/wiki/函数)运算，并且避免使用[程序状态](https://zh.wikipedia.org/w/index.php?title=程式状态&action=edit&redlink=1)以及[易变对象](https://zh.wikipedia.org/wiki/不可變物件)。其中，[λ演算](https://zh.wikipedia.org/wiki/Λ演算)（lambda calculus）为该语言最重要的基础。而且，λ演算的函数可以接受函数当作输入（引数）和输出（传出值）。
>
> 比起[指令式编程](https://zh.wikipedia.org/wiki/指令式編程)，函数式编程更加强调程序执行的结果而非执行的过程，倡导利用若干简单的执行单元让计算结果不断渐进，逐层推导复杂的运算，而不是设计一个复杂的执行过程。
>
> 在函数式编程中，函数是[第一类对象](https://zh.wikipedia.org/wiki/第一类对象)，意思是说一个函数，既可以作为其它函数的参数（输入值），也可以从函数中返回（输入值），被修改或者被分配给一个变量。

**简单来说:**函数式是一种编程形式，你可以将函数作为参数传递给其他函数，并将它们作为值返回。 在函数式编程中，我们以函数的形式思考和编程。

JavaScript，Haskell，Clojure，Scala 和 Erlang 是部分实现了函数式编程的语言。



# 一等函数

如果你在学习 JavaScript，你可能听说过 JavaScript 将函数视为一等公民。 那是因为在 JavaScript 及其他函数式编程语言中，**函数是对象**。

在 JavaScript 中，函数是一种特殊类型的对象。 它们是 Function objects。例如：

```javascript
function sayHello(){
    console.log("Hello");
}
sayHello();   // 控制台上打印 'Hello'
```

为了更进一步的证明js中的函数是对象:

```javascript
sayHello.name='My Name is SayHello'
console.log(sayHello.name)   // 控制台打印 'My Name is SayHello'
```

**注意** - 虽然这在 JavaScript 中完全有效，但这被认为是 harmful 的做法。 你不应该向函数对象添加随机属性，如果不得不这样做，请使用对象。

在 JavaScript 中，你对其他类型（如对象，字符串或数字）执行的所有操作都可以对函数执行。 你可以将它们作为参数传递给其他函数（回调函数），将它们分配给变量并传递它们等等。这就是 JavaScript 中的函数被称为一等函数的原因。



## 将函数赋值给变量

我们可以在 JavaScript 中将函数赋值给变量。 例如：

```javascript
const square = function (z){
    return z * z
}
console.log(square(5))  // 控制台打印 '25'
```

我们也可以将该方法进行传递:

```javascript
const temp = square;
console.log(temp(6))   //控制台打印36
```



## 将函数作为参数传递

```javascript
function sayHello(){
    console.log("Hello");
}

function sayBye(){
    console.log("Bye");
}

function say(type,sayHello,sayBye){
    if(type === 1){
        // 根据就近原则,此处使用的是方法参数中的sayHello
        sayHello();
    }else{
        // 通上
        sayBye();
    }
}

say(1,sayHello,sayBye); //控制台打印 'Hello'
```



# 高阶函数

> 在[数学](https://zh.wikipedia.org/wiki/数学)和[计算机科学](https://zh.wikipedia.org/wiki/计算机科学)中，**高阶函数**是至少满足下列一个条件的[函数](https://zh.wikipedia.org/wiki/函数)：
>
> - 接受一个或多个函数作为输入
> - 输出一个函数
>
> 在数学中它们也叫做[算子](https://zh.wikipedia.org/wiki/算子)（运算符）或[泛函](https://zh.wikipedia.org/wiki/泛函)。[微积分](https://zh.wikipedia.org/wiki/微积分)中的[导数](https://zh.wikipedia.org/wiki/导数)就是常见的例子，因为它映射一个函数到另一个函数。
>
> 在[无类型lambda演算](https://zh.wikipedia.org/wiki/Lambda演算)，所有函数都是高阶的；在[有类型lambda演算](https://zh.wikipedia.org/wiki/有类型lambda演算)（大多数[函数式编程语言](https://zh.wikipedia.org/wiki/函数式编程语言)都从中演化而来）中，高阶函数一般是那些函数型别包含多于一个箭头的函数。在函数式编程中，返回另一个函数的高阶函数被称为[Curry化](https://zh.wikipedia.org/wiki/Curry化)的函数。
>
> 在很多函数式编程语言中能找到的`map`函数是高阶函数的一个例子。它接受一个函数**f**作为参数，并返回接受一个列表并应用**f**到它的每个元素的一个函数。
>
> 高阶函数的其他例子包括[函数复合](https://zh.wikipedia.org/wiki/函数复合)、[积分](https://zh.wikipedia.org/wiki/积分)和常量函数λ*x*.λ*y*.*x*。

高阶函数是对其他函数进行操作的函数，操作可以是将它们作为参数，或者是返回它们。 简单来说，高阶函数是一个接收函数作为参数或将函数作为输出返回的函数。

例如，Array.prototype.map，Array.prototype.filter 和 Array.prototype.reduce 是语言中内置的一些高阶函数。



## JavaScript内置高阶函数

### filter

**作用: **filter用于对数组进行过滤。它创建一个新数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

**注意:** 

- filter()不会对空数组进行检测
- filter()不会改变原始数组



**语法**

```javascript
Array.filter(function(currentValue, indedx, arr), thisValue)
```

第一个参数为函数

这个回调函数的返回值是一个boolean值

当检查元素符合过滤条件时,返回true,函数内部会自动将这个元素加入新数组中

当不符合条件时会返回false，函数内部会过滤掉这个元素