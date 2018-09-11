---
title: 使用RVM安装管理Ruby
date: 2018-08-07 23:36:00
tags: [Ruby]
---

> RVM(Ruby Version Manager)是一个负责安装、管理不同版本的Ruby环境的命令行工具。

### 安装RVM

- 确认是否已安装`curl`，没有则执行命令`yum install curl`进行安装；
- 打开[RVM官网](https://www.rvm.io/)，使用上面提供的命令安装RVM：
```shell
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
$ \curl -sSL https://get.rvm.io | bash -s stable
```
- 使配置文件生效：
  - 查找配置文件位置： `find / -name rvm.sh`
  - 配置文件生效： `source /etc/profile.d/rvm.sh`
- 下载RVM依赖： `rvm requirements`

### 安装Ruby
- 查看RVM库所有Ruby版本： `rvm list known`
- 指定安装其中某个版本的Ruby ：`rvm install ruby-2.5.1`

### 常用命令
- 查看所有已安装的Ruby版本信息：`rvm list`
- 设置Ruby的当前需要版本： `rvm use 2.5.1`
- 设置为默认版本：`rvm use 2.5.1 default`
- 删除其中某个版本：`rvm remove 2.5.1`
