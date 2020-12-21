---
title: Dockerfile常用语法
date: 2018-02-22 03:27:00
tags: [Docker]
---

> 介绍Dockerfile部分较为常用的语法，关于Docker常用命令参考[Docker常用命令](https://www.0x0f0f.com/Docker/frequently-used-docker-command-line/)。

- `FROM ${IMAGE}[:${TAG}]`
  - 仅且必须出现在Dockerfile的第一个指令，指定构建镜像的基础源镜像；
  - 不填TAG时默认TAG为`latest`；

- `MAINTAINER ${AUTHOR} ${EMAIL}`
  - 记录构建镜像的作者及其邮箱的信息；

- `RUN ${COMMAND} ${PARAM}..`
  - 每执行一次`RUN`都会形成一层新的镜像，因为镜像只读，在后续命令里删除、卸载等操作无法影响前面已形成的镜像，因此体积也不会下降；
  - 因为每一个`RUN`命令都会形成新的镜像，因此可以通过`&&`符号连接多条命令一次完成；
  - 执行的命令不继承原来的环境变量；

- `CMD ${COMMAND} ${PARAM}..`
  - Dockerfile启动时的提供的默认命令；
  - 只能有一个`CMD`，写多个时最后一个生效，启动容器时用户提供了命令的话则此项会被覆盖而失效；

- `ENTRYPOINT ${COMMAND} ${PARAM}..`
  - 同`CMD`，但不会被用户启动容器时指定的`CMD`命令覆盖，如果要覆盖需要指定OPTION `--entrypoint`；
  - 同样只能有一个`ENTRYPOINT`，写多个时最后一个生效；

- `EXPOSE ${CONTAINER_PORT}`
  - 指定容器需要与外部进行映射的端口，允许使用多个`EXPOSE`进行多端口映射，在启动添加OPTION`-P`时生效；

- `ENV ${KEY}=${VALUE}`
  - 设置容器的环境变量，也可以提供给后面的`RUN`命令使用；
  - 如需要一行设置多个环境变量时可以用空格分隔；

- `ARG ${KEY}`
  - 设置构建镜像时需要使用的参数，`ARG ${KEY}=${VALUE}`可以设置默认值；
  - 使用方式为在`docker build`时通过`--build-arg ${KEY}=${VALUE}`传入参数；

- `ADD ${SOURCE} ${DEST}`
  - 把本地或者远程文件或文件夹复制到容器中；
  - 如果传输的是压缩包则会自动解压；

- `COPY ${SOURCE} ${DEST}`
  - 功能同`ADD`，但不能处理网络文件，也不会自动解压压缩文件；

- `VOLUME ${PATH}`
  - 指定容器内要挂载到主机的目录；
  - 使用该方式时对应主机的目录是自动生成的，无法被指定；
  - 可以在使用`docker run`启动容器时通过OPTION`-v`的方式代替该方式；

- `USER ${USER_NAME}`
  - 指定运行容器的用户；

- `WORKDIR ${PATH}`
  - 为后续的`RUN`、`CMD`、`ENTRYPOINT`命令指定容器内的工作目录；

- `ONBUILD ${INSTRUCTION}`
  - 配置该镜像被作为其它镜像的基础镜像时需要执行的命令；
