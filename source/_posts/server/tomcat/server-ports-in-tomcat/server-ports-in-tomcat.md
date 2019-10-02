---
title: Tomcat的端口作用
date: 2017-07-11 19:15:00
tags: [Server]
---

> `Apache Tomcat`是我们最常使用到的应用服务器，一般来说，Tomcat默认的配置已经足以支撑我们正常的使用，无需做更多的修改。但是当一台机器上需要启动多个Tomcat服务的时候，我们会发现服务因为端口冲突而启动失败，这时候我们就需要手动修改配置文件中的端口配置了。

Tomcat所使用到的端口主要包括4个，默认分别为8005、8080、8009和8443，其配置都在Tomcat目录的`conf/server.xml`里，下面分别对几个端口的作用进行说明：

- `8005`：Tomcat服务监听关闭操作的端口，多个服务时需要修改该端口以防止服务在其他Tomcat服务关闭的同时被关闭；

```xml
<Server port="8005" shutdown="SHUTDOWN">
```

- `8080`：Tomcat的连接端口；

```xml
<Connector port="8080" protocol="HTTP/1.1"
        connectionTimeout="20000"
        redirectPort="8443" server="APP Srv1.0"/>
```

- `8009`：Tomcat接受其他服务器转发过来的请求；

```xml
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
```

- `8443`：上面8080和8009端口接收到SSL请求的时候重定向的端口，修改时以上两处都要同时修改；
