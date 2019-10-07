---
title: ElasticSearch集群搭建
date: 2017-08-28 23:46:30
tags: [ElasticSearch]
---

> 由于ElasticSearch不同版本的配置有所不同，特指出本文所用版本为5.5.2.

## 安装配置

### 一、建立Linux用户【ES不允许直接使用root帐号】

```bash
groupadd app
useradd -g app elastic
passwd elastic
      [pw:e1asticsearch]

mkdir /elastic
chown -R elastic:app /elastic
chmod 755 /elastic
```

建立用户并授权目录后使用新的用户进行后续操作。

### 二、下载解压并放到适当位置

解压命令：

`tar -zxvf elasticsearch-5.5.2.tar.gz`

### 三、修改配置文件

修改配置文件参数：

`vim ./elasticsearch-5.5.2/config/elasticsearch.yml`

```bash
# 集群名称，同一集群内必须设置相同的名称
cluster.name: 自定义
# 节点名称，同一集群内设置不同名称
node.name: 自定义
# host地址，可以为0.0.0.0，一般直接设置为本机IP
network.host: 本机IP
# 默认端口9200
http.port: 9200
# 集群内的机器IP，需要添加集群内节点通讯端口，端口默认为9300, 如 "127.0.0.1:9300"
discovery.zen.ping.unicast.hosts: ["","",""]
```

### 四、启动

使用一下命令启动，可添加参数-d后台启动：

```bash
./bin/elasticsearch [-d]
```

## 各类报错

### 一、 root用户启动

- 报错信息：`java.lang.RuntimeException: can not run elasticsearch as root`

  解决方法：设置一个非root用户，用于管理elastic

### 二、 节点数不足

- 报错信息：`max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]`
  
  解决方法：

  ```bash
  $ ulimit -Hn  # 查看硬限制
  $ vim /etc/security/limits.conf
      # 添加以下两行【elastic是用户名】：
      elastic soft nofile 65536
      elastic hard nofile 65536
  ```

  重新登录检查是否生效。

- 报错信息：`max number of threads [1024] for user [elastic] is too low, increase to at least [2048]`

   解决方法：

  ```bash
  $ vim /etc/security/limits.d/xx-nproc.conf
  #【文件名会有所不同,且节点数不一定为1024，大于需求值就不需要改】
    # 修改：
    * soft nproc 1024 -> * soft nproc 2048
  ```

- 报错信息：`max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]`

  解决方法：

  ```bash
  $ vim /etc/sysctl.conf
      # 添加：
      vm.max_map_count=655360
  # 检查：
  $ sysctl -p
  ```

### 三、 使用公网IP无法访问/无法绑定路由

- 原因：防火墙未开放端口

  解决方法（此处举例为CentOS7）：

  ```bash
  # 添加9200端口开放传输：
  $ firewall-cmd --zone=public --add-port=9200/tcp --permanent
  # 添加9200端口开放集群通讯：
  $ firewall-cmd --zone=public --add-port=9300/tcp --permanent
  # 重启防火墙：
  $ firewall-cmd --reload
  ```
