---
title: Effeective Java - 创建和销毁对象
copyright: true
mathjax: true
date: 2020-11-12 13:39:40
categories:
- 笔记
- 后端
tags:
- Java
- Effective Java
---

​		本篇博客的主题是创建和销毁对象,何时以及如何创建对象,何时以及如果避免创建对象,如何确保他们能够适时地销毁,以及如何管理对象之间必须进行的各个清理动作。

​		以下内容为读```《Effective Java》```后，对书中的内容进行总结，欢迎补充。

<!-- less -->



# 1.使用静态工厂方法替代构造器

​		对于一个类而言，为了让客户端获取它自身的一个实例，最传统的方法就是提供一个公有的构造器。还有一种方法，也应该在每个程序员的工具箱中占有一席之地。类可以提供一个公有的静态工厂方法（static factory method）,它只是一个返回类的实例的静态方法。下面是一个`Boolean`(boolean类型的装箱类)的简单示例。这个方法将boolean基本类型值转换成了一个`Boolean`的对象引用。

```java
public static final Boolean TRUE = new Boolean(true);
public static final Boolean FALSE = new Boolean(false);

public static Boolean valueOf(boolean b) {
        return (b ? TRUE : FALSE);
}
```

​		**注意：静态工厂方法和设计模式中的工厂方法模式不同。此处所指的静态工厂方法并不直接对应于设计模式中的工厂方法。**



## 1.1.静态工厂相较于构造器优点

- **静态工厂方法与构造器不同的第一大优势在于，它们有名称。**

    ​		如果构造器的参数本身没有确切地描述正被返回的对象，那么具有适当名称的静态工厂会更容易使用，产生的客户端代码也更易于阅读。

    ​		例如：构造器`BigInteger(int,int,Random)`返回的BigInteger可能为素数，如果用名为`BigInteger.probablePrime`的静态工厂方法来表示，显然更为清楚。（Java 4 版本中增加了该方法）

    ​		一个类只能有一个带有指定标签的构造器（参数个数相同，类型相同，顺序相同）。但是如果需要两个相同参数类型，相同个数的构造器，那么也就只能将顺序改变，这样导致的后果就是，用户永远也记不住该用哪个构造器，结果常常会调用错误的构造器。并且在读到使用了这些构造器的代码时，如果没有参考文档，往往不知所云。 

- **静态工厂方法与构造器不同的第二大优势在于，不必再每次调用它们的时候都创建一个新对象。**

    ​		这使得不可变类可以使用预先构建好的实例，或者将构建好的实例缓存起来，进行重复利用，从而避免创建不必要的重复对象。比如上述例子中使用的`Boolean.valueOf(boolean)`方法说明了这项技术：它从来不创建对象。这种方法类似于享元模式。如果程序经常请求创建相同的对象，并且创建对象的代价很高，则这项技术可以极大地提升性能。

    ​		静态工厂方法能够重复的调用返回相同对象，这样有助于类总能严格控制在某个时刻哪些实例应该存在。这种类被称为`实例受控的类（instance-controlled）`。编写实例受控的类有几个原因。实例受控使得类可以确保它是一个Singleton或者是不可实例化的。它还使得不可变的值类可以确保不会存在两个相等的实例，即当`a==b`时，`a.equals(b)`才为`true`。这是享元模式的基础。枚举（enum）类型保证了这一点。

- **静态工厂方法与构造器不同的第三大优势在于，它们可以返回原返回类型的任何子类型的对象。**

    ​		这样使我们在选择返回对象的类时就有了更大的灵活性。

    ​		这种灵活性的一种应用时，API可以返回对象，同时又不会使对象的类变成公有的。以这种方式隐藏实现类会使API变得更加简洁。这项技术适用于*基于接口的框架（interface-based framwork）*，因为在这种框架中，接口为静态工厂方法提供了自然返回类型。

- **静态工厂的第四大优势在于，所返回的对象的类可以随着每次调用而发生变化，这取决于静态工厂方法的参数值。**

- **静态工厂的第五大优势在于，方法返回的对象所属的类，在编写包含该静态工厂方法的类时可以不存在。**

    ​		这种灵活的静态工厂方法构成了服务提供者框架（Service Provider Framework）的基础，例如JDBC（Java数据库连接）API。服务提供者框架是指这样一个系统：多个服务提供者实现一个服务，系统为服务提供者的客户端提供多个实现，并把它们从多个实现中解耦出来。

    ​		服务提供者框架中有三个重要的组件：①服务接口（Service Interface），这是提供者实现的；②提供者注册API（Provider Registration API），这是提供者用来注册实现的；③服务访问API（Service Access API），这是客户端用来获取服务的实例。服务访问API是客户端用来指定某种选择实现的条件。如果没有这样的规定，API机会返回默认实现的一个实例，或者允许客户端遍历所有可用的实现。服务访问API时“灵活的静态工厂”，它构成了服务提供者框架的基础。  

    ​		服务提供者框架的第四个组件服务提供者接口（Service Provider Interface）是可选的，它表示产生服务接口之实例的工厂对象。如果没有服务提供者接口，实现就通过反射方式进行实例化。对于JDBC来说，Connnection就是其服务接口的一部分，`DriverManager.registerDriver`是提供者注册API，`DriverManager.getConnection`是服务访问API，Driver是服务提供者接口。



## 1.2.静态工厂相较于构造器缺点

- **静态工厂方法的主要缺点在于，类如果不含公有的或者受保护的构造器，就不能被子类化。**

- **静态工厂方法的第二个缺点在于，程序员很难发现它们。**

    ​		在API文档中，它们没有想构造器那样在API文档中明确的标识出来，因此，对于提供了静态工厂方法而不是构造器的类来说，要想查明如何实例化一个类时非常困难的。



## 1.3.静态工厂的惯用名称

- from  类型转换方法，它只有单个参数，返回该类型的一个相对应的实例，例如：

    ```java
    Date d = Date.from(instant);
    ```

- of  聚合方法，带有多个参数，返回该类型的一个实例，把它们合并起来，例如：

    ```java
    Set<Rank> faceCards = EnumSet.of(JACK,QUEEM,KING);
    ```

- valueOf  比from和of更繁琐的一种替代方法，例如：

    ```java
    BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
    ```

- instance  或者getInstance   返回的实例是通过方法的（如有）参数来描述的，但是不能说与参数相同的值，例如：

    ```java
    StackWalker luke = StackWalker.getInstance(options);
    ```

- create  或者  newInstance  像instance或者getInstance一样，但create或者newInstance能够确保每次调用都返回一个新的实例，例如：

```java
Object newArray = Array.newInstance(classObject,arrayLen)
```

- getType  像getInstance一样，但是在工厂方法处于不同的类中的时候使用。Type 表示工厂方法所返回的对象类型，例如：

```java
FileStore fs = Files.getFileStore(path);
```

- newType 像newInstance一样，但是在工厂方法处于不同的类中的时候使用。Type表示工厂方法所返回的对象雷翔，例如：

```java
BufferedReader br = Files.newBufferedReader(path);
```

- type  getType和newType的简版，例如：

```java
List<Complaint> litany = Collections.list(legacyLitany);
```



## 总结

​		静态工方法和公有构造器都各有用处，我们需要理解它们各自的长处。静态工厂经常更加合适，因此切忌第一反应就是提供公有构造器，而不先考虑静态工厂。





# 2.遇到多个构造器参数时要考虑使用构建器



