---
title: JDK源码分析-ArrayList
date: 2016-02-01 20:23:00
tags: [Java,JDK]
---

## 基本概述

- `ArrayList`的本质是数组，占用一块连续的内存空间，可以动态扩容；
- 没有实现同步，并非线程安全的；
- 实现了接口`RandomAccess`，支持通过下标进行快速随机访问；
- 实现了接口`Cloneable`，可以被克隆；
- 实现了接口`Serializable`，支持序列化；

## 构造方法

三种构造方法：

```java
public ArrayList(); // 无参构造
public ArrayList(int initialCapacity); // initialCapacity为初始化数组的长度大小
public ArrayList(Collection<? extends E> c); // 用Collection对象的元素来构建
```

特别提一下的是在JDK1.6中，无参构造的数组默认长度是10，是调用带数组长度的构造方法来初始化的：

```java
// JDK1.6
public ArrayList() {
    this(10);
}
```

而在JDK1.7、1.8中，无参构造则是直接赋予一个空数组（即默认长度为0），在调用`add`方法的时候才做扩容：

```Java
// JDK1.8
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

## 动态扩容

当数组的实际长度将要比当前预留的要大时，需要对数组进行扩容。

上面提到新版本JDK的无参构造是直接赋予一个空数组，而调用`add`方法时，会判断变更后长度是否比默认长度小，是的话则设置数组长度为`DEFAULT_CAPACITY`，即默认最小长度10：

```java
private static final int DEFAULT_CAPACITY = 10; // 默认最小长度为10
private void ensureCapacityInternal(int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        // 如果小于默认长度10，则取默认长度
        minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity); 
    }
    ensureExplicitCapacity(minCapacity);
}
```

而在实际添加元素时空间不够会调用`grow()`方法来调整数组长度，先约定扩大长度为当前的1.5倍，依然不够的话则设置为实际需要的长度：

```java
private void grow(int minCapacity) {
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1); // 扩容为原来的1.5倍
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity; // 如果依然不够，则直接设置为需要的大小
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

由上面可以得知，每次扩容数组都会执行一次`Arrays.copyOf(elementData, newCapacity);`，但`ArrayList`的扩容算法使这些频繁的复制操作基本都集中在前面数据量较小处，到了后期数据量大的时候复制操作相对比较少了，所以设置一个合理的长度虽然可以减少数据复制的次数，但也只能“稍微”提高一点性能，反而设置了过大的长度还可能会浪费内存。如何取舍看具体场景。

另外，`ArrayList`有一个设置数组长度为当前实际长度的`trimToSize()`方法：

```java
public void trimToSize() {
    modCount++;
    if (size < elementData.length) {
        elementData = (size == 0) ? EMPTY_ELEMENTDATA : Arrays.copyOf(elementData, size);
    }
}
```

## 线程安全

`ArrayList`没有实现同步，所以不是线程安全的。代码文档中提到，`ArrayList`类大致等同于`Vector`类，而`Vector`是线程安全的，所以是可以使用`Vector`来做多线程的实现。另外也提到可以使用`Collections.synchronizedList`方法将`ArrayList`“包装”起来：

```java
List list = Collections.synchronizedList(new ArrayList(...));
```

当然，因为`Collections.synchronizedList`实质上底层存储的还是构造时传进来的参数`list`，只是对它做了一层包装，所以对数据操作起来会比使用`Vector`慢一点点。Oracle文档里的原话：

> If you need synchronization, a Vector will be slightly faster than an ArrayList synchronized with Collections.synchronizedList.
