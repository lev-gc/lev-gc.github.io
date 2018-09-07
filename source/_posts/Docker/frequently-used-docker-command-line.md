---
title: Docker常用命令
date: 2018-02-21 18:20:00
tags: [Docker]
---

> 列举部分较为常用的Docker命令行指令。

## 镜像相关命令

> 对特指的某个镜像操作时必须要指定该镜像的名称和TAG，如果不指定TAG会默认TAG为`latest`，另外也可以通过镜像ID来直接指定。

- `docker images` : 查看当前本地仓库所有镜像

- `docker search ${KEYWORD}` : 在线查找镜像

- `docker login [${SERVER}]` : 登录某个Docker仓库，默认为官方仓库DockerHub
    - `-u ${USERNAME}` : 指定登录用户名
    - `-p ${PASSWORD}` : 指定登录用户的密码

- `docker logout [${SERVER}]` : 退出登录

- `docker pull ${IMAGE}:${TAG}` : 下载镜像，不指定TAG则默认下载`latest`版本

- `docker push ${IMAGE}:${TAG}` : 上传镜像到镜像仓库，需要先登陆镜像仓库

- `docker rmi ${IMAGE}:${TAG}` : 删除本地仓库中镜像

- `docker save ${IMAGE}:${TAG} -o ${FILE_NAME}` : 把镜像保存为tar文件，也可以写成`docker save ${IMAGE}:${TAG}>${FILE_NAME}`

- `docker load -i ${FILE_NAME}` : 把镜像文件导入到仓库，也可以写成`docker load <${FILE_NAME}`

- `docker export ${CONTAINER_ID} -o ${FILE_NAME}` : 把容器保存为tar文件，也可以写成`docker export ${IMAGE}:${TAG}>${FILE_NAME}`

- `docker import ${FILE_NAME} ${IMAGE}:${TAG}` : 把镜像文件导入到仓库，需要指定导入后镜像的IMAGE和TAG

- `docker tag ${IMAGE}:${TAG} ${NEWIMAGE}:${NEWTAG}` : 为某个镜像增加新的镜像名和TAG

- `docker commit ${CONTAINER_ID} ${IMAGE}:${TAG}` : 提交对某个容器的更改操作，输出为新的镜像
    - `-m="${COMMIT_MESSAGE}"` : 填写提交信息
    - `-a="${AUTHOR}"` : 填写镜像作者
    - `-p` : commit时暂停容器

- `docker build ${DOCKER_FILE_PATH} -t ${IMAGE}:${TAG}` : 通过Dockerfile创建镜像

## 容器运行相关命令

> 对容器进行操作时需要指定该容器的ID，另外也可以通过分配的或自定义的容器名来确定容器。

- `docker run` : 启动一个新的容器
    - `-t` : 在新容器内指定一个伪终端或终端
    - `-i` : 允许对容器内的标准输入(STDIN)进行交互
	- `-d` : 后台运行
	- `-P` : 选取主机的随机端口对容器端口进行映射
	- `-p ${HOST_PORT}:${CONTAINER_PORT}` : 指定端口的映射，可以使用多次`-p`以映射多个端口
	- `--name ${CONTAINER_NAME}` : 指定容器的NAME
	- `-v ${HOST_PATH}:${CONTAINER_PATH}` : 把本机某个目录挂载到容器的某个目录下

- `docker stop ${CONTAINER_ID}` : 停止某个Docker容器

- `docker start ${CONTAINER_ID}` : 启动某个关掉的Docker容器

- `docker restart ${CONTAINER_ID}` : 重启某个正在运行的Docker容器

- `docker kill ${CONTAINER_ID}` : 强行停止某个Docker容器

- `docker attach ${CONTAINER_ID}` : 进入某个正在后台运行的容器，通过`CTRL+P+Q`可以把容器再次切换回后台

- `docker exec ${CONTAINER_ID} ${COMMAND}` : 在某个运行中的容器里执行COMMAND后退出，如果要进入容器则COMMAND为`/bin/bash`

- `docker pause ${CONTAINER_ID}` : 暂停容器

- `docker unpause ${CONTAINER_ID}` : 恢复被暂停的容器

- `docker ps` : 查看当前正在运行的容器的信息
    - `-a` : 查看包括已关闭的容器
    - `-q` : 只列出容器的ID，可以与其他需要容器ID的命令组合使用，如[docker rm `docker ps -a -q`]可以删除全部的容器

- `docker port ${CONTAINER_ID}` : 查看容器的端口映射情况

- `docker logs ${CONTAINER_ID}` : 查看容器内的标准输出
    - `-f` : tail形式输出log

- `docker top ${CONTAINER_ID}` : 查看容器内部运行的进程

- `docker inspect ${CONTAINER_ID}` : 查看容器的底层信息

- `docker rm ${CONTAINER_ID}` : 移除不需要的容器

- `docker diff ${CONTAINER_ID}` : 查看容器发生改变的文件结构

- `docker cp ${HOST_PATH} ${CONTAINER_ID}:${CONTAINER_PATH}` : 本机和容器间的文件传输，在前的为源路径，在后面的为目标路径，容器侧的路径前要加上容器ID

## Docker自身服务相关命令

- `docker version` : 查看当前使用的Docker版本信息

- `docker info` : 查看Docker当前的详细系统信息
