---
title: Nacos安装指引
date: 2020-08-30 17:56:44
tags: [middleware]
---

## 说明

- Nacos提供的应用包是跨平台通用的版本，其中包含了`.sh`和`.cmd`的脚本，主程序是一个jar包，本文档以Linux安装使用为例进行说明；
- 安装版本：[nacos-1.3.2](https://github.com/alibaba/nacos/releases/download/1.3.2/nacos-server-1.3.2.zip)；
- 安装目录：`/opt/nacos`；

## 安装步骤

### 1. 检查运行环境

- Nacos主程序是一个jar包，需要在JAVA环境下运行，需要先检查是否已经配置好环境；

### 2. 下载解压

- 下载应用包：`wget https://github.com/alibaba/nacos/releases/download/1.3.2/nacos-server-1.3.2.zip`；
- 解压：`unzip nacos-server-1.3.2.zip`；
- 移动文件夹到`/opt`目录下：`sudo mv nacos /opt/nacos`；

### 3. 修改配置

由于Nacos默认以集群的方式运行的，当需要单体运行的时候，需要在启动脚本的时候添加额外的参数`-m standalone`，但经常输入额外参数会比较麻烦，因此可以直接修改启动脚本`bin/startup.sh`进行以下修改即可默认以单体的方式启动：

```sh
export MODE="cluster"
# 修改为
export MODE="standalone"
```

### 4. 操作命令

- Nacos启动命令：`sh /opt/nacos/bin/startup.sh`；
- Nacos停止命令：`sh /opt/nacos/bin/shutdown.sh`；

如使用的系统为`Ubuntu`一类请替换`sh`为`bash`。

### 5. 验证启动及使用

- 启动Nacos后，控制台会输入提示查看`/opt/nacos/logs/start.out`文件确认启动是否完成；
- Nacos的默认端口为`8848`，可以通过浏览器访问地址`http://${IP}:8848/nacos/`打开Nacos的监控面板；
- Nacos提供注册中心服务的地址为`nacos://${ip}:8848`；
