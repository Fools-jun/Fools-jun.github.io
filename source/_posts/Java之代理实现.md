---
title: Java之代理实现
date: 2020-10-14 14:11:16
categories:
- 笔记
- 后端
tags:
- Java
copyright: true
---

深入浅出分析代理模式（未完）

<!-- less -->



# 代理模式

- **定义：**给目标对象提供一个代理对象，并由代理对象控制对目标对象的引用。
- **目的：**
    1. 通过引入代理对象的方式来间接访问目标对象，防止直接访问目标对象个系统带来的不必要复杂性；
    2. 通过代理对象对原有的业务进行增强；



## 静态代理

![静态代理类图](https://gitee.com/junpzx/blog-img/raw/master//img/20201014151602.png)

### 适用场景

代理类较少且确定



## 动态代理



![动态代理类图](https://gitee.com/junpzx/blog-img/raw/master//img/20201014151934.png)



### 适用场景

适用于大部分场景，相较于静态代理而言，动态代理更加灵活，易扩展，如果需要扩展，只需要新增接口和类就行





# 代理实现代码

## 静态代理

**抽象接口：定义了代理对象和真实对象都要实现的公共接口**

```java
/***
 * @author Pzx
 * @date 2020/10/20 14:06
 * @desc 
 */
public interface Subject {

    /**
     * 售卖化妆品
     */
    void saleCosmetics();
}

```



**真实对象：代理对象所代表的真实对象，最终被引用的对象**

```java
/***
 * @author Pzx
 * @date 2020/10/20 14:08
 * @desc 大宝化妆品公司
 */
public class ProxyDaBaoCompany implements Subject{

    @Override
    public void saleCosmetics() {
        System.out.println("大宝化妆品公司卖化妆品");
    }
}
```



**代理对象：包含真实对象从而操作真实对象，相当与`用户（张三）和真实对象（大宝化妆品公司）之间的中介（小鑫）。**

```java
/***
 * @author Pzx
 * @date 2020/10/20 14:12
 * @desc 小鑫个人代理
 */
public class ProxyXiaoXin implements Subject{

    private ProxyDaBaoCompany daBao;


    public ProxyXiaoXin(ProxyDaBaoCompany daBao){
        this.daBao = daBao;
    }

    @Override
    public void saleCosmetics() {
        System.out.println("小鑫售前服务");
        daBao.saleCosmetics();
        System.out.println("小鑫售后服务");
    }
}
```



**测试类**

```java
/***
 * @author Pzx
 * @date 2020/10/20 14:19
 * @desc
 */
public class Test {
    public static void main(String[] args) {
        ProxyXiaoXin proxyXiaoXin = new ProxyXiaoXin(new ProxyDaBaoCompany());
        proxyXiaoXin.saleCosmetics();
    }
}
```



**运行结果**

```
小鑫售前服务
大宝化妆品公司卖化妆品
小鑫售后服务
```



## 动态代理

### JDK原生动态代理（ASM实现）









### CGLib动态代理（ASM实现）







### Javassist动态代理