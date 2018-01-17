---
title: 关于BigDecimal
date: 2015-11-22 18:19:00
tags: [Java]
---
> `java.math.BigDecimal`是由任意精度的整数非标度值和32位的整数标度(`scale`)组成的不可变的、任意精度的有符号十进制数。在计算的时候`BigDecimal`提供了精确的数值计算，所以也理所当然地经常会需要进行数值的舍入。

### 赋值：

1. 传入字符串的赋值方式：

  `new BigDecimal("1.22");`

2. 调用`valueOf(double)`方法的赋值方式：

  `BigDecimal.valueOf(1.22);`

3. JDK描述对参数类型为`double`的构造方法结果有一定的不可预知性，大部分情况下这种构造方式的结果不会正好等于传入的`double`值：

  `new BigDecimal(1.22);` = `1.2199999999999999733546474089962430298328399658203125`

### 舍入模式 (RoundingMode)：

1. *ROUND_UP*

   远离零的舍入模式，即绝对值始终增大。

2. *ROUND_DOWN*

   接近零的舍入模式，即绝对值始终减小。

3. *ROUND_CEILING*

   接近正无穷大的舍入模式，即原始值始终增大。

4. *ROUND_FLOOR*

   接近负无穷大的舍入模式，即原始值始终减小。

5. *ROUND_HALF_UP*

   向最接近的数字舍入，如果距两边数字差值相等则向上舍入，即绝对值四舍五入。

6. *ROUND_HALF_DOWN*

   向最接近的数字舍入，如果距两边数字差值相等则向下舍入，即绝对值五舍六入。

7. *ROUND_HALF_EVEN*

   向最接近的数字舍入，如果距两边数字差值相等则向偶数边舍入，即绝对值四舍六入，五靠偶数。

8. *ROUND_UNNECESSARY*

   不做舍入，具有断言的作用，指定后如出现需要舍入的情况则会抛出`ArithmeticException`异常。

### 运算：

```java
public BigDecimal add(BigDecimal augend);                       //加法
public BigDecimal subtract(BigDecimal subtrahend);              //减法
public BigDecimal multiply(BigDecimal multiplicand);            //乘法
public BigDecimal divide(BigDecimal divisor, int roundingMode); //除法，一般都需要指定舍入模式
```