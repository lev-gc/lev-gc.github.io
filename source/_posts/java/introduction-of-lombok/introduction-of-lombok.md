---
title: Lombok介绍
date: 2017-03-14 19:32:50
tags: [Java]
---

## 简介

Lombok是一个能通过添加简单的注解来代替一些必须有但会让代码整体变得臃肿的代码，然后在编译的时候才把注解转换成对应代码的工具。

GitHub项目：[Lombok](https://github.com/rzwitserloot/lombok)

项目官网：[HomePage](https://projectlombok.org/)

## 部分注解说明

| 注解 | 说明 |
| :-- | :-- |
| @Getter/@Setter | 可以用来标注Class或者Field，会为非静态Field提供Getter和Setter方法 |
| @ToString | 生成toString方法，默认会输出Class名和按顺序用逗号分隔输出Field |
| @EqualsAndHashCode | 默认为所有非transient和非static的Field生成Equals和HashCode方法，也能指定Field |
| @NonNull | 标注Field不能为null，会在Setter方法和构造方法里添加非空判断 |
| @NoArgsConstructor | 注解在Class上，生成一个无参构造方法 |
| @RequiredArgsConstructor | 注解在Class上，生成一个包含常量和标注了@NonNull的Field的参数的构造方法 |
| @AllArgsConstructor | 注解在Class上，生成一个全参的构造方法 |
| @Data | 包括@ToString、@EqualsAndHashCode、所有Field的@Getter、所有非final的Field的@Setter、@RequiredArgsConstructor几个注解的组合 |
| @Log系列注解 | 使用不同类型的Log包来为标注的Class生成静态常量log |

以上为比较常用的一些注解，当然还有其他的注解，以及部分注解可以通过添加参数或者修改配置文件的方式来修改生成代码的效果，具体可以到官网查阅。

## 一些个人看法

在某种程度上，Lombok是一个比较方便的工具，可以简化一些臃肿的代码，但是这并不是一个完美的工具，需要慎用。

- 本工具的价值体现在编译前代码的简洁性上，对编译后的代码只会是有增无减，因此并不会为代码的运行效率带来提升，甚至会造成下降；
- 在使用IDE进行开发时依赖于Lombok插件，如果没有安装插件代码会提示报错；
- 并不便于开发人员阅读代码，尤其是不熟悉本工具的开发人员更是容易对代码产生疑惑；
- 配置文件的生效方式类似于`EditorConfig`，如不做好管控容易造成混乱；
- 即使是有多种参数和配置文件，也不一定能生成完全符合想法的代码，滥用注解会让代码更加凌乱；
