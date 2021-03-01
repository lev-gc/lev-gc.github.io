---
title: Linux上配置Java环境
date: 2016-08-11 15:20:00
tags: [Java,Linux]
---

## 说明

- 通过yum、apt等包管理工具安装的一般是openjdk，如果需要oracle jdk的话需要到oracle官网下载包进行安装；
- 安装Java环境一般分为两种，安装JDK和安装JRE： 
  - 如果不需要进行开发、编译等操作，只用Java环境来运行一些Jar、War包的软件的话可以只安装JRE环境；
  - JDK额外提供了一些开发工具包，如果需要进行编译操作，如使用javac、Maven等执行编译，则应该安装JDK环境；

## 配置过程

此处以安装oracle jdk为例：

- 下载
  - 官方下载链接需要认证，不能直接用wget下载，可以先通过浏览器下载再上传到对应位置；

- 解压
  - 解压命令：`tar -zxvf jdk-8u201-linux-x64.tar.gz`；
  - 移动到对应目录，一般可以放在`/usr/local/`目录下；

- 环境配置
  - 编辑配置文件： `vim /etc/profile`；
  - 添加以下配置（根据实际情况修改路径）：

    ```bash
    export JAVA_HOME=/usr/local/jdk1.8.0_201
    export PATH=$JAVA_HOME/bin:$PATH
    ```

    说明：Java1.5版本以后就不需要配置`CLASSPATH`了。

- 使环境变量配置生效并检验是否配置成功
  - 使用`source`命令使之生效：`source /etc/profile`；
  - 查看配置结果：`echo $JAVA_HOME`；

    ![效果图](https://raw.githubusercontent.com/lev-gc/lev-gc.github.io/source/source/_posts/java/install-of-java-environment-on-linux/java_env.png)
