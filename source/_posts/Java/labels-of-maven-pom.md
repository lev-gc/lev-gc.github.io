---
title: Maven POM文件常用标签解释
date: 2016-11-10 23:43:00
tags: [Java,Maven]
---

> 本文档仅描述POM文件中最常用的一部分标签，完整的标签说明请查阅[官方文档](http://maven.apache.org/ref/3.2.5/maven-model/maven.html)。
> 另外，项目查找依赖包的优先级：local > setting-profiles > pom-repositories > setting-mirror/central

### 基本标签
| Label | Description |
| :--: | ---- |
| modelVersion | 声明所使用POM模型的版本 |
| parent | 项目的父模块，需要填写父模块的`groupId`、`artifactId`和`version`；另外`relativePath`是设置父模块的位置，设置后会优先查找该目录而非Maven仓库 |
| groupId | 模块所属的group的唯一id，设置`parent`后可以不填`groupId`，会默认使用`parent`的`groupId` |
| artifactId | 本模块的唯一ID |
| version | artifact的版本号 |
| packaging | 本模块打包类型，可选`jar`、`pom`、`war`、`ear`，默认为`jar` |
| name/url/description | 为描述项目而设置的标签，非必须 |

#### - modules
列出本项目的子模块，使用相对路径。

#### - repositories
| Label | Description |
| ---- | ---- |
| releases | 设置是否下载release的包 |
| snapshots | 设置是否下载snapshot的包，一般公共仓库都设置为`false` |
| id | 仓库ID，需要和setting文件中的`server`的ID配置相同 |
| name | 仓库名称 |
| url | 仓库的地址 |
| layout | Maven1的`layout`是`legacy`，默认是`default` |

####  - pluginRepositories
配置基本和远程一样，但是查找插件时会到这里的插件仓库照而不是到repositories配置的仓库中找。

#### - distributionManagement
通过deploy命令把构建的包分发到远程仓库。
| Label | Description |
| ---- | ---- |
| snapshotRepository | 快照仓库 |
| repository | release仓库 |
| id/name/url | 仓库的配置 |

此外，一般还需要在setting文件配置对应id的server：

```xml
<servers>
    <server>
        <id>repo1</id>
        <username>repouser</username>
        <password>my_login_password</password>
        <!-- To encrypt passwords: http://maven.apache.org/guides/mini/guide-encryption.html -->
    </server>
</servers>
```

### - dependencyManagement
负责管理相关依赖的版本等信息。本标签下的dependency只管理信息而不直接加载依赖，当本POM文件或者继承的POM文件中的dependencies标签中的dependency只指定了groupId和artifactId而没有描述version、scope等信息，就会默认从dependencyManagement中查找相关信息。

### - dependencies
负责加载项目的相关依赖。
| Label | Description |
| ---- | ---- |
| groupId | 依赖包所在group的ID |
| artifactId | 依赖包的ID |
| version | 依赖包的版本号 |
| groupId | 依赖包所在group的ID |
| exclusions | 配置此项来排除依赖，去除本依赖包所依赖的其他包 |
| classifier | 用来指定依赖包有多个输出包时应该使用哪一个，如有多个由不同jdk编译出来的版本时需要指定使用哪一个版本 |
| systemPath | 当依赖包在本地而非仓库时，`sytemPath`指出了jar包的路径 |
| type | 指出所依赖的包的类型，默认为jar |
| optional | 表示其他项目依赖本项目时，是否传递该依赖。`true`表示不传递，默认为`false` |
| scope | 打包时对依赖包的不同操作模式 |

下面是scope的几种模式：
- compile: 默认值，表示当前依赖需要参与到项目的编译、测试、运行，打包时会被include进去；
- test: 表示当前依赖仅参与测试的工作，包括测试代码的编译和执行，打包时不会被include进去；
- runtime: 表示当前依赖不参与项目的编译，但会参与代码的测试和执行，打包时会被include进去；
- provided: 相当于`compile`，但打包时不会include该依赖，即使运行需要也是由外部环境提供；
- system: 相当于`provided`，但是该依赖由本地文件系统提供，需要添加`systemPath`定义所在路径；

### - plugins
Maven本质上是一个插件框架，它的核心并不执行任何具体的构建任务，所有这些任务都交给插件来完成。通过plugins的配置可以完成一些额外的构建任务，或者修改构建过程所使用的插件的版本。

### - profiles
指定基于不同环境参数的构建所使用的各种参数的值。

### - build
指定构建过程中的各种自定义工作。
| Label | Description |
| ---- | ---- |
| sourceDirectory | Java文件目录 |
| testSourceDirectory | 测试Java文件目录 |
| resources | 资源文件目录 |
| testResources | 测试资源文件 |
| outputDirectory | 源文件输入出目录   |
| testOutputDirectory | 测试文件输出目录 |
| finalName | 指出最终打包的包名 |
| defaultGoal | 指定默认的参数，当执行maven命令时没有指定参数是使用默认参数，如`compile`、`install` |
| filters | 使用配置文件中的值替换项目中的占位符 |

### - properties
通过key-value的方式自定义一些参数。
