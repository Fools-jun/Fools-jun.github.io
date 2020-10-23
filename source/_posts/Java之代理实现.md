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
        super();
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

### JDK动态代理

JDK是通过反射生成一个实现代理接口的匿名类，调用InvocationHandler来处理。



#### 代码

##### 抽象接口

定义了代理对象和真实对象都要实现的公共接口

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



##### 真实对象1

代理对象所代表的真实对象，最终被引用的对象

```java
/***
 * @author Pzx
 * @date 2020/10/20 14:08
 * @desc 大宝化妆品公司
 */
public class DaBaoCompany implements Subject{

    @Override
    public void saleCosmetics() {
        System.out.println("大宝化妆品公司卖化妆品");
    }
}
```



##### 真实对象2

代理对象所代表的真实对象，最终被引用的对象

```java
/***
 * @author Pzx
 * @date 2020/10/20 14:08
 * @desc 百雀羚化妆品公司
 */
public class BaiQueLingCompany implements Subject{

    @Override
    public void saleCosmetics() {
        System.out.println("百雀羚化妆品公司卖化妆品");
    }
}

```



##### 代理对象

包含真实对象从而操作真实对象

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

/***
 * @author Pzx
 * @date 2020/10/20 14:38
 * @desc 小鑫代理公司
 */
public class ProxyXiaoXinCompany implements InvocationHandler {
	
    private Subject subject;

    public ProxyXiaoXinCompany(Subject subject){
        super();
        this.subject = subject;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("小鑫代理公司售前服务");
        Object result = method.invoke(subject, args);
        System.out.println("小鑫代理公司售后服务");
        return result;
    }
    
    
     /**
     * 获取目标对象的代理对象
     * @return 代理对象
     */
    public Object getProxy() {
        return Proxy.newProxyInstance(Thread.currentThread().getContextClassLoader(),
                subject.getClass().getInterfaces(), this);
    }
}
```



##### 测试类

```java
import java.lang.reflect.Proxy;

/***
 * @author Pzx
 * @date 2020/10/20 14:44
 * @desc 测试类
 */
public class Test {
    public static void main(String[] args) {
        System.out.println("--------------testA start--------------");
        testA();
        System.out.println("--------------testA end--------------");
        System.out.println("--------------testB start--------------");
        testB();
        System.out.println("--------------testB end--------------");
        System.out.println("--------------testC start--------------");
        testC();
        System.out.println("--------------testC end--------------");
    }


    public static void testA(){
        // 首先确定用户需要买什么牌子的化妆品
        ProxyXiaoXinCompany proxyXiaoXinCompany = new ProxyXiaoXinCompany(new DaBaoCompany());
        // 如果买大宝，那么就找一个代理公司中专门负责大宝的代理员工
        Subject subject = (Subject)Proxy.newProxyInstance(
                // 代理对象的类加载器
                proxyXiaoXinCompany.getClass().getClassLoader(),
                // 代理对象需要实现接口
                new Class<?>[]{Subject.class},
                // 方法调用的实际处理者，代理对象的方法调用都会转发到这里
                proxyXiaoXinCompany);
        // 卖化妆品
        subject.saleCosmetics();
    }

    public static void testB(){
        ProxyXiaoXinCompany proxyXiaoXinCompany = new ProxyXiaoXinCompany(new BaiQueLingCompany());
        Subject subject = (Subject) proxyXiaoXinCompany.getProxy();
        subject.saleCosmetics();
    }
    
    
    public static void testC(){
        Subject subject = (Subject) Proxy.newProxyInstance(BaiQueLingCompany.class.getClassLoader(), new Class[]{Subject.class}, (proxy, method, args) -> {
            System.out.println("前置增强");
            try {
                return method.invoke(new BaiQueLingCompany(),args);
            }finally {
                System.out.println("后置增强");
            }
        });
        subject.saleCosmetics();
    }
    
}
```



##### 运行结果

```
--------------testA start--------------
小鑫代理公司售前服务
大宝化妆品公司卖化妆品
小鑫代理公司售后服务
--------------testA end--------------
--------------testB start--------------
小鑫代理公司售前服务
百雀羚化妆品公司卖化妆品
小鑫代理公司售后服务
--------------testB end--------------
--------------testC start--------------
前置增强
百雀羚化妆品公司卖化妆品
后置增强
--------------testC end--------------
```



用起来是很简单吧，其实这里基本上就是AOP的一个简单实现了，在目标对象的方法执行之前和执行之后进行了增强。Spring的AOP实现其实也是用了Proxy和InvocationHandler这两个东西的。

用起来是比较简单，但是如果能知道它背后做了些什么手脚，那就更好不过了。首先来看一下JDK是怎样生成代理对象的。既然生成代理对象是用的Proxy类的静态方newProxyInstance，那么我们就去它的源码里看一下它到底都做了些什么？

#### 代码分析

```java
/** 
 * 返回指定接口的代理类的实例，该实例将方法调用分派给指定的调用*处理程序
 *
 * loader:类加载器 
 * interfaces:目标对象实现的接口 
 * h:InvocationHandler的实现类 
 */ 
public static Object newProxyInstance(ClassLoader loader,
                                          Class<?>[] interfaces,
                                          InvocationHandler h)
        throws IllegalArgumentException
    {
    	// 判断InvocationHandler的实现类是否为null，为null则抛出NullPointerException
        Objects.requireNonNull(h);
		// 克隆出接口类
        final Class<?>[] intfs = interfaces.clone();
    	// 获取Java安全管理器
        final SecurityManager sm = System.getSecurityManager();
        if (sm != null) {
            // 检查创建代理类所需的权限
            checkProxyAccess(Reflection.getCallerClass(), loader, intfs);
        }
		// 查找或生成指定的代理类
        Class<?> cl = getProxyClass0(loader, intfs);

    	// 用指定的调用处理程序调用其构造函数
        try {
            if (sm != null) {
                checkNewProxyPermission(Reflection.getCallerClass(), cl);
            }
			// 调用代理对象的构造函数（也就是$Proxy0(InvocationHandler h))
            final Constructor<?> cons = cl.getConstructor(constructorParams);
            final InvocationHandler ih = h;
            // 判断生成的代理类的访问修饰符是否为public
            if (!Modifier.isPublic(cl.getModifiers())) {
                AccessController.doPrivileged(new PrivilegedAction<Void>() {
                    public Void run() {
                        cons.setAccessible(true);
                        return null;
                    }
                });
            }
            return cons.newInstance(new Object[]{h});
        } catch (IllegalAccessException|InstantiationException e) {
            throw new InternalError(e.toString(), e);
        } catch (InvocationTargetException e) {
            Throwable t = e.getCause();
            if (t instanceof RuntimeException) {
                throw (RuntimeException) t;
            } else {
                throw new InternalError(t.toString(), t);
            }
        } catch (NoSuchMethodException e) {
            throw new InternalError(e.toString(), e);
        }
    }
```



```java
/**
 * 生成代理类。必须先调用checkProxyAccess方法才能执行权限检查
 */
private static Class<?> getProxyClass0(ClassLoader loader,
                                           Class<?>... interfaces) {
    	// 如果说接口的数量大于65535,报错
        if (interfaces.length > 65535) {
            throw new IllegalArgumentException("interface limit exceeded");
        }

        // 如果存在由实现了给定接口的给定加载器定义的代理类，则将仅返回缓存的副本；
    	// 否则，它将通过ProxyClassFactory创建代理类
        return proxyClassCache.get(loader, interfaces);
    }
```



```java
/**
 * key：ClassLoader
 * paramter: 接口数组
 */
public V get(K key, P parameter) {
    	// 要求paramter不能为空
        Objects.requireNonNull(parameter);
		// 删除过期元素
        expungeStaleEntries();
		// 构建一个 WeakReference 对象(key 表示 ClassLoader)
        Object cacheKey = CacheKey.valueOf(key, refQueue);

        // 懒加载的方式封装第二层valueMap，ConcurrentMap是一种线程安全的Map
        ConcurrentMap<Object, Supplier<V>> valuesMap = map.get(cacheKey);
        if (valuesMap == null) {
            ConcurrentMap<Object, Supplier<V>> oldValuesMap
                = map.putIfAbsent(cacheKey,
                                  valuesMap = new ConcurrentHashMap<>());
            if (oldValuesMap != null) {
                valuesMap = oldValuesMap;
            }
        }

        // 创建次级 Key
    	// 同时检索是否存在过去有同一个 ClassLoader 加载的表示 Supplier<V>
        Object subKey = Objects.requireNonNull(subKeyFactory.apply(key, parameter));
        Supplier<V> supplier = valuesMap.get(subKey);
        Factory factory = null;

        while (true) {
            if (supplier != null) {
                 // 最终 supplier 将非空，同时调用 .get() 方法获取 Class 类
                V value = supplier.get();
                if (value != null) {
                    return value;
                }
            }
            // else no supplier in cache
            // or a supplier that returned null (could be a cleared CacheValue
            // or a Factory that wasn't successful in installing the CacheValue)
            // 未找到过去加载的记录

           // 懒加载一个 Factory 
            if (factory == null) {
                factory = new Factory(key, parameter, subKey, valuesMap);
            }

            if (supplier == null) {
                supplier = valuesMap.putIfAbsent(subKey, factory);
                if (supplier == null) {
                    // successfully installed Factory
                    supplier = factory;
                }
                // else retry with winning supplier
            } else {
                if (valuesMap.replace(subKey, supplier, factory)) {
                    // successfully replaced
                    // cleared CacheEntry / unsuccessful Factory
                    // with our Factory
                    supplier = factory;
                } else {
                    // retry with current supplier
                    supplier = valuesMap.get(subKey);
                }
            }
        }
    }
```









### CGLib动态代理（ASM实现）







### Javassist动态代理





# 总结





## 面试经常会问到的问题

###1.动态代理有什么作用及应用场景？

1. 日志集中打印
2. 事务
3. 权限管理
4. AOP



### 2.在SpringAOP当中有哪些方式实现及区别？

- Java Proxy
    - 动态构建字节码：动态构建全新字节码bean，初始化的时候
- cglib
    - 动态构建字节码：动态构建全新字节码bean，初始化的时候
- Aspectj
    - 修改目标类的字节码，织入代理的字节，在程序编译的时候
    - 编译的时候插入动态代理的字节码，不会生成全新的Class
- instrumentation
    - 修改目标类的字节码、类装载的时候动态拦截去修改，基于javaagent
        - javaagent:spring-instrument-4.3.8.RELEASE.jar
    - 类装载的时候插入动态代理的字节码，不会生成全新的Class



### 3.Java Proxy,cglib,Aspectj,instrumentation的相同点和不同点

- 其本质都是对字节码的修改，其区别就是从哪里切入修改字节码

