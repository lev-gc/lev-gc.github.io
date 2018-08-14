---
title: Linux上配置Java环境
date: 2016-08-11 15:20:00
tags: [Java,Linux]
---
## **简单描述一下Linux下配置Java环境的流程。**

1. 下载
  - 解压版比较灵活，这里我选择了下载tar版
  - Linux下可以通过wget命令加下载链接直接下载
  - 当然也可以先下载，然后通过scp命令上传到Linux上

2. 解压
  - 通过命令解压：`tar -zxvf jre1.8.0_144.tar.gz`
  - 移动到对应目录，这里我把它放在`/usr/home/java`目录下

3. 环境配置
  - 编辑配置文件： `vim /etc/profile`
  - 添加以下配置（根据实际情况修改路径）：
```bash
JAVA_HOME=/usr/home/java
export JRE_HOME=/usr/home/java/jre1.8.0_144
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

4. 使环境变量配置生效并检验是否配置成功
  - 重启Linux使环境变量生效：`reboot`
  - 若不重启也可直接使用`source`命令使之生效：`source /etc/profile`
  - 查看配置结果：`echo $JAVA_HOME`

![效果图](https://raw.githubusercontent.com/lev-gc/lev-gc.github.io/source/source/_posts/Java/environment-of-java-on-linux/result.png)