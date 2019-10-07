---
title: EditorConfig
date: 2017-12-14 19:47:00
tags: [Lint]
---

[EditorConfig](http://editorconfig.org/) 是一套用于统一代码风格的方案，可以帮助开发者在不同的编辑器和IDE之间定义和维护一致的代码风格，同时有利于团队的合作，避免不必要的格式重构。

## 原理

当打开一个文件时，`EditorConfig`插件会从该文件所在目录开始往上每一级目录中查找`.editorconfig`文件，直到找到`.editorconfig`文件中有`root=true`这个配置为止，并把该配置文件作为根配置。`EditorConfig`插件会从根配置文件开始开始往下层读取，下层文件配置会覆盖上层文件配置，所以最接近所打开文件的配置优先级最高。另外，配置文件里的属性名和属性值大小写不敏感，编译时都会被转为小写，而没有被明确指定的某个属性则会使用编辑器默认的配置。

## 通配符说明

|    通配符     | 作用                                |
| :--------: | :-------------------------------- |
|     *      | 匹配除/之外的任意字符串(即不跨目录，特殊字符可以用`\` 转义) |
|     **     | 匹配任意单个字符(即遍历子目录下的文件) |
|     ?      | 匹配任意单个字符 |
|   [name]   | 匹配name字符 |
|  [!name]   | 匹配非name字符 |
| {s1,s3,s3} | 匹配任意给定的字符串 |

## 通用属性

|            属性          | 说明                                      |
| :----------------------: | ---------------------------------------- |
|       indent_style       | [tab/space] |
|       indent_size        | 对应indent_style=space时的列数 |
|        tab_width         | 对应indent_style=tab时的列数，默认等于indent_size |
|       end_of_line        | [lf/crlf/cr] lf(\n)为Unix和OS X的默认值/crlf(\r\n)为Windows的默认值/cr(\r)为Classic Mac OS的默认值 |
|         charset          | [utf-8/latin1/gbk/etc..] |
| trim_trailing_whitespace | [true/false] 是否将行尾空格自动删除 |
|   insert_final_newline   | [true/false]是否使文件以一个空白行结尾 |
|           root           | [true]表明是最顶层的配置文件，发现设为true时，才会停止继续查找 |

> 设置为unset时可以把配置重置为编辑器默认配置，另外有一些适配不同编辑器的属性，详见[wiki](https://github.com/editorconfig/editorconfig/wiki/EditorConfig-Properties) 。
