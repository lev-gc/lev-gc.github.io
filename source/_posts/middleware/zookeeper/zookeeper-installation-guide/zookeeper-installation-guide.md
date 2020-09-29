---
title: zookeeper安装指引
date: 2018-11-07 17:44:30
tags: [middleware]
---

## 说明

- [apache-zookeeper](https://github.com/apache/zookeeper)提供了跨平台的应用包，本文档以Linux安装使用为例进行说明；
- 版本：[zookeeper-3.5.8](https://github.com/apache/zookeeper/archive/release-3.5.8.tar.gz)
- 安装目录：`/opt/zookeeper`

## 安装步骤

### 检查运行环境

- zookeeper需要JAVA环境，需要先检查是否已经配置；

### 下载解压

- 下载安装包：`wget https://github.com/apache/zookeeper/archive/release-3.5.8.tar.gz`；
- 解压：`tar -zxvf apache-zookeeper-3.5.8-bin.tar.gz`；
- 移动文件夹到`/opt`目录下：`sudo mv zookeeper-3.4.8 /opt/zookeeper`；

### 修改配置

- `/opt/zookeeper`目录下的`conf`目录为配置文件目录，把其中的`zoo_sample.cfg`复制一份作为zookeeper的启动配置：`cp /opt/zookeeper/conf/zoo_sample.cfg /opt/zookeeper/conf/zoo.cfg`；
- 可修改配置项：

    ```conf
    # 修改数据保存文件
    dataDir=/opt/zookeeper/data/

    # 默认端口为2181，如有必要可更改以下配置项
    clientPort=2181
    ```

### 启停命令

- zookeeper启动命令：`/opt/zookeeper/bin/zkServer.sh start /opt/zookeeper/conf/zoo.cfg`；
- zookeeper状态检查：`/opt/zookeeper/bin/zkServer.sh status`；
- zookeeper关闭命令：`/opt/zookeeper/bin/zkServer.sh stop`；
- zookeeper重启命令：`/opt/zookeeper/bin/zkServer.sh restart`；

### 配置开机启动

修改文件：`vim /etc/rc.local`，追加以下行（请修改`<USER>`为启动时期望使用的Linux用户）：

`su - <USER> -c /opt/zookeeper/bin/zkServer.sh start /opt/zookeeper/conf/zoo.cfg &`
