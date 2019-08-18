---
title: Linux上配置Java环境
date: 2016-08-11 15:20:00
tags: [Java,Linux]
---

## 配置过程

- 下载
  - 解压版比较灵活，所以选择下载tar包；
  - 官方下载链接需要认证，不能直接用wget下载，可以先通过浏览器下载再上传到对应位置；

- 解压
  - 解压命令：`tar -zxvf jdk-8u201-linux-x64.tar.gz`；
  - 移动到对应目录，一般放在`/usr/local/`目录下；

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

    ![效果图](https://raw.githubusercontent.com/lev-gc/lev-gc.github.io/source/source/_posts/java/environment-of-java-on-linux/java_env.png)
