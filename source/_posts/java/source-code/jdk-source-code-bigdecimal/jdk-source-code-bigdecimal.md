---
title: JDK源码分析-BigDecimal
date: 2015-11-22 18:19:00
tags: [Java,JDK]
---

> `java.math.BigDecimal`是由任意精度的整数非标度值和32位的整数标度(`scale`)组成的不可变的、任意精度的有符号十进制数，表示的数值是 `unscaledValue × 10^(-scale)` 。在计算的时候`BigDecimal`提供了精确的数值计算，所以也理所当然地经常会需要进行数值的舍入。

## 赋值

- 传入字符串的赋值方式：

```java
  new BigDecimal("1.22");
```

- 调用`valueOf(double)`方法的赋值方式：

```java
  BigDecimal.valueOf(1.22);
```

- JDK源码注释里描述对参数类型为`double`的构造方法要求参数是`double` 的二进制浮点值准确的十进制表示形式，因此直接传入double值结果有一定的不可预知性，大部分情况下这种构造方式的结果不会正好等于传入的`double`值（因为不能表示为任何有限长度的二进制小数）：

```java
  new BigDecimal(1.22);
  // = 1.2199999999999999733546474089962430298328399658203125
```

## 舍入模式 (RoundingMode)

- *ROUND_UP*

   远离零的舍入模式，即绝对值始终增大。

- *ROUND_DOWN*

   接近零的舍入模式，即绝对值始终减小。

- *ROUND_CEILING*

   接近正无穷大的舍入模式，即原始值始终增大。

- *ROUND_FLOOR*

   接近负无穷大的舍入模式，即原始值始终减小。

- *ROUND_HALF_UP*

   向最接近的数字舍入，如果距两边数字差值相等则向上舍入，即绝对值四舍五入。

- *ROUND_HALF_DOWN*

   向最接近的数字舍入，如果距两边数字差值相等则向下舍入，即绝对值五舍六入。

- *ROUND_HALF_EVEN*

   向最接近的数字舍入，如果距两边数字差值相等则向偶数边舍入，即绝对值四舍六入，五靠偶数。

- *ROUND_UNNECESSARY*

   不做舍入，具有断言的作用，指定后如出现需要舍入的情况则会抛出`ArithmeticException`异常。

## 基本运算

```java
public BigDecimal add(BigDecimal augend);                       //加法
public BigDecimal subtract(BigDecimal subtrahend);              //减法
public BigDecimal multiply(BigDecimal multiplicand);            //乘法
public BigDecimal divide(BigDecimal divisor, int roundingMode); //除法，一般都需要指定舍入模式
```
