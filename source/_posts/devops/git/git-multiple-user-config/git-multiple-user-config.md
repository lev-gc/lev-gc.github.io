---
title: Git配置多用户
date: 2016-05-22 16:52:00
tags: [DevOps,Git]
---

## 场景描述

对于使用Git仓库，我们可能需要在同一个系统中使用不同的邮箱账号进行开发工作，比如工作时需要使用工作邮箱和易于识别的名字，而社区则是使用私人邮箱和网络名称，如果只使用一套全局配置会很不方便。Git提供了处理这个问题的机制。

## 原理

基础配置本质上存在于两个配置文件中：

- 全局配置，一般存放在用户目录下的`.gitconfig`文件中；
- 局部配置，存放在仓库目录下的`.git/config`文件中；

优先级上，__局部配置大于全局配置__。Git提供的`git config`命令本质上就是改变这两个文件的配置，其中加了`--global`参数的命令改动的是全局配置，而没有加该参数的则是改变对应仓库的局部配置。因此我们可以先全局配置一个相对常用的用户，然后再在不使用该用户信息的仓库里进行局部配置。

## 相关操作

### 方法一：命令行操作

```bash
# 配置用户名和邮箱
git config [--global] user.name "xxx"
git config [--global] user.email "xxx"

# 取消全局设置
git config [--global] --unset user.name
git config [--global] --unset user.email
```

### 方法二：直接编辑配置文件

除了命令的方式外，当然也可以直接编辑`.gitconfig`或者`.git/config`：

```config
[user]
    name = xxx
    email = xxx
```
