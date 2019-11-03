---
title: 设计模式 - 单例模式
date: 2016-06-28 23:41:00
tags: [Design Pattern]
---

> 单例模式是指一个类只能构建一个对象的设计模式。单例模式根据构建对象的时间分为两类：启动时就构建好的饿汉式和初次被调用才构建的懒汉式。实现一个单例模式主要需要考虑的是线程安全问题，另外是否能懒加载和是否能防止反射构建也是需要考虑的一部分。

### 双重检测锁实现

双重检测锁是单例模式的一种实现，可以满足线程安全和懒加载的需要，不过不能防止利用反射来构建多个对象。

#### 基本实现

首先我们从最简单的代码开始一步步构建双重检测锁的单例实现：

```java
public class Singleton {

    private Singleton() {} //构造函数私有化
    private static Singleton instance = null; //单例对象

    //静态初始化方法
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

- 首先第一步，我们是把构造方法给私有化，防止外部随意调用构造方法构建对象；
- `instance`便是我们的单例对象，因为是一个静态成员，赋值为`null`实现了懒加载，当然也可以写成`new Singleton()`实现饿汉式加载；
- `getInstance()`是获取单例对象的方法，通过`if (instance == null)`这一步来确保对象只会被构建一次。

#### 确保线程安全

上面的基本实现能保证在单线程下实现单例模式，但在多线程的情况下可能会出现一些问题：
刚开始`instance`还是等于`null`的时候，`if (instance == null)`这一步判断是为`true`，当线程A通过了这一步判断进入了分支里但还没来得及构建对象的时候，`instance`实际上还是为`null`，这时候线程B依然能通过判断进入分支体，于是就会出现多个线程同时在分支体里面，导致分支体里的`instance = new Singleton();`会被执行多次，构建了多次对象。

添加双重检测机制：

```java
public class Singleton {

    private Singleton() {} //构造函数私有化
    private static Singleton instance = null; //单例对象

    //静态初始化方法
    public static Singleton getInstance() {
        if (instance == null) {                 //双重检测机制
            synchronized (Singleton.class) {    //同步锁
                if (instance == null) {         //双重检测机制
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

- 通过添加同步锁`synchronized`的方式，保证同一时刻最多只会有一个线程在执行同步锁里的程序，保证`instance = new Singleton();`只会被执行一次；
- 同步锁外的`if (instance == null)`判空是指当不是`null`的时候就直接跳过整个分支体，而不用等待完同步锁后才发现`instance`不为`null`，减少不必要的时间开销；
- 同步锁里的`if (instance == null)`判空是为了防止线程A进入了锁里面还没来得及创建对象时，线程B就通过了外部判空开始等待锁的情况出现，这种机制就叫做双重检测锁。

### 禁止指令重排序

上面的实现看似保证了线程安全，但实际上并非如此，原因在于Java指令的重排序。
什么是指令重排序？上面构建对象部分的代码`instance = new Singleton();`，实际上会被编译器编译成以下指令：

1. 分配对象需要的内存空间
2. 在分配的内存中初始化对象
3. 把`instance`指向上面分配到的内存地址

指令流水线并非是串行的，多条指令可以同时被执行，为了使性能更优，JVM和CPU可能会把这些指令进行优化重排序，导致出现以下的顺序：

1. 分配对象需要的内存空间
2. 把`instance`指向上面分配到的内存地址
3. 在分配的内存中初始化对象

结果会导致这样一种情况：线程A正在执行`instance = new Singleton()`，获取了内存地址但是还没有初始化对象，而此时线程B在外部判空时发现`instance`不等于`null`，于是直接返回了`instance`，便造成线程B拿到了一个还没完成初始化的对象。

添加修饰符volatile：

```java
public class Singleton {

    private Singleton() {} //构造函数私有化
    private volatile static Singleton instance = null; //单例对象

    //静态初始化方法
    public static Singleton getInstance() {
        if (instance == null) {                 //双重检测机制
            synchronized (Singleton.class) {    //同步锁
                if (instance == null) {         //双重检测机制
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

使用volatile修饰符禁止了指令重排序，保证指令是按顺序执行的，不会出现一种中间态。另外一提，volatile关键字除了禁止指令重排外，还能确保其修饰的值被修改后马上写回到主内存中，保证线程访问的该变量值是最新值。

至此为止，代码已经实现了线程安全且懒加载的单例模式，然而，硬要用反射来使构造函数可见，还是能构建多个对象的。
