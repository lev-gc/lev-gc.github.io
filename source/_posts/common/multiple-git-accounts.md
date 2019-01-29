---
title: Git多账号配置
date: 2016-05-22 16:52:00
tags: [Git]
---

### 场景
有时候，我们可能需要在同一台电脑环境下用不同的邮箱账号进行Git的相关工作，比如工作需要使用工作邮箱和易于识别的名字，而社区则是使用私人的邮箱和网络名称，如果只有一套配置会很不方便。

### 原理
Git的基础配置本质上就存在于两个配置文件中：
- 全局配置，存放在用户目录下的`.gitconfig`文件中；
- 局部配置，存放在仓库目录下的`.git/config`文件中；

优先级上，__局部配置大于全局配置__。Git提供的`git config`命令本质上就是改变这两个文件的配置，其中加了`--global`参数的命令改动的是全局配置，而没有加该参数的则是改变对应仓库的局部配置。

### 相关操作

#### 一、用命令的方式操作

```bash
# 配置用户名和邮箱
git config [--global] user.name "xxx"
git config [--global] user.email "xxx"
# 取消全局设置
git config [--global] --unset user.name
git config [--global] --unset user.email
```

#### 二、直接编辑的方式
除了命令的方式外，当然也可以直接编辑`.gitconfig`或者`.git/config`：
```
[user]
    name = xxx
    email = xxx
```