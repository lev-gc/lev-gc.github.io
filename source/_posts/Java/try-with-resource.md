---
title: Java7对流类资源关闭的自动执行
date: 2016-03-08 10:06:10
tags: [Java]
---

对于流类，有3个相当重要的接口：`Closeable`、`Flushable`、`AutoColseable`，其中`AutoColseable`是JDK7新添加的接口，这个接口让我们对流类的异常处理的有了新的写法。

本来对于流类，由于代码出现异常时，异常后面的代码不会被执行，通常会导致后面对资源的关闭操作无法被执行，从而会出现不可预料的错误，因此我们必须把资源关闭的操作写到`finally{}`里以保证代码会被执行：

```java
try {
    // 1. resource registered
    // 2. do something
  } catch (IOException e) {
    // exception handler
  } finally {
    // 3. resource close
  }
```

但是Java7为我们优化了对这种问题的处理方式，我们只需把流类资源的申请放到try后面的小括号里即可，不必进行显式的资源关闭，`try`语句可以自动执行资源关闭，即便在`try`代码块中发生了异常：

```java
try(
    // 1. resource registered [auto close]
){
    // 2. do something
}catch(IOException e){
    // exception handler
}
```

当然，能使用这种方式的必须是实现了`java.lang.AutoCloseable`的类。