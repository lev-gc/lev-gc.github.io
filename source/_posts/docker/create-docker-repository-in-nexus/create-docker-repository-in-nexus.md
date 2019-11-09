---
title: Nexus上配置Docker仓库
date: 2018-08-15 22:25:00
tags: [Docker]
---

> 本文主要介绍如何在Nexus上配置Docker的仓库，以及如何配置本地Docker使用搭建好的Docker仓库。

## 环境

- 服务器的Nexus版本为3.x以上，可下载安装，也可以直接使用Docker镜像的版本；
- 使用方的系统及Docker版本没有限制，但因为不同版本的系统能兼容的Docker版本区别较大，此处仅以Centos7搭配Docker17为例说明；

## 关于Docker仓库

Nexus中的Docker仓库分为三种类型：

| Type | Description |
| :--: | ---- |
| proxy | 代理仓库，提供代理其他仓库，一旦下载过的镜像会被存储在该仓库，下次无需再从外部网络重新下载 |
| hosted | 私有仓库，提供本地私服功能，上传的镜像可与使用该Nexus的用户共享 |
| group | 组合仓库，实际为多个仓库的组合，但只能使用原来仓库存在的镜像，不能直接通过对组合仓库操作来添加镜像 |

## 配置步骤

- 首先，打开Nexus的系统页面，按图中步骤开始创建仓库：

  ![点击创建仓库](https://raw.githubusercontent.com/lev-gc/lev-gc.github.io/source/source/_posts/docker/create-docker-repository-in-nexus/create_repositories.png)

- 选择不同Docker仓库类型后进入各自的创建页面：

  ![选择仓库类型](https://raw.githubusercontent.com/lev-gc/lev-gc.github.io/source/source/_posts/docker/create-docker-repository-in-nexus/select_recipe.png)

  - proxy

    ![proxy仓库配置](https://raw.githubusercontent.com/lev-gc/lev-gc.github.io/source/source/_posts/docker/create-docker-repository-in-nexus/proxy_repository_configure.png)

    关键步骤：

    - 输入唯一的仓库名称；
    - 选择协议及对应端口，该端口会在后面配置Docker镜像时会用到，注意所选端口不要被占用；
    - 填写本仓库所代理的远程仓库，可使用国内的镜像进行加速；

  - hosted

    ![hosted仓库配置](https://raw.githubusercontent.com/lev-gc/lev-gc.github.io/source/source/_posts/docker/create-docker-repository-in-nexus/hosted_repository_configure.png)

    关键步骤：

    - 输入唯一的仓库名称；
    - 选择协议及对应端口，会在配置Docker登录、上传下载私服仓库镜像时会用到，注意所选端口不要被占用；

  - group

    ![group仓库配置](https://raw.githubusercontent.com/lev-gc/lev-gc.github.io/source/source/_posts/docker/create-docker-repository-in-nexus/group_repository_configure.png)

    关键步骤：

    - 输入唯一的仓库名称；
    - 选择协议及对应端口，该端口会在后面配置Docker镜像时会用到，注意所选端口不要被占用；
    - 选择该组合仓库需要包括的仓库，镜像会从所选的仓库里获取；

## 配置使用Nexus的Docker仓库

- 新建或修改配置文件`/etc/docker/daemon.json`，添加

  ```json
  {
    "registry-mirrors": ["http://xxx.xxx.xxx.xxx:8099"],
    "insecure-registries":["http://xxx.xxx.xxx.xxx:8098"]
  }
  ```

  其中：
  - `http://xxx.xxx.xxx.xxx:8099`为Nexus上的proxy仓库或group仓库，配置生效后会默认通过该仓库获取镜像；
  - `http://xxx.xxx.xxx.xxx:8098`为私服仓库，即镜像push的目标仓库，使用前需要先通过`docker login`登陆；
  - 注意`registry-mirrors`的地址必须要加上协议类型[http/https]；

- 使配置生效

  ```bash
  systemctl daemon-reload
  systemctl restart docker
  ```

- 登录Nexus私服帐号

  ```bash
  docker login xxx.xxx.xxx.xxx:8098
  # 使用Nexus帐号登录，Nexus默认帐号密码: admin/admin123
  ```

## 使用Nexus仓库

- 使用Nexus的proxy仓库

  当配置好环境后默认即使用Nexus的proxy仓库，每次本地pull新的镜像时，proxy仓库就会下载并缓存该镜像，下次再pull的时候就会直接从proxy仓库取而不必再通过外部网络获取。

- 使用Nexus的hosted仓库

  对于自构建的镜像，需要通过Nexus的hosted仓库上传下载，以下为具体步骤：

  - 使用`docker tag`命令对构建完的镜像进行命名，命名规则为在镜像名前添加hosted仓库地址（即`xxx.xxx.xxx.xxx:8098`），如`java:1.8`应该命名为`xxx.xxx.xxx.xxx:8098/java:1.8`，因为如果不重命名则会默认提交到Docker Hub上；
  - 确认已登录后把重命名的镜像上传，如 `docker push xxx.xxx.xxx.xxx:8098/java:1.8`；
  - push成功后可在Nexus上查看到上传的镜像；
  - 需要使用时通过`docker pull xxx.xxx.xxx.xxx:8098/java:1.8`命令可从hosted仓库下载该镜像；
