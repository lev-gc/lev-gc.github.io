---
title: Linux上搭建Shadowsocks
date: 2017-05-03 17:00:00
tags: [Shadowsocks,Linux]
---
- ## 说明	

  ​为了搭建过境的通道，首先需要一台服务器/VPS，IP在境外的，Linux系统，CentOS或者Ubuntu都行，配置不需要多高，最基本的就行，重点是与自己本地的网络连接延时要低，可以先ping一下ip测试下。Linode等都有提供测试节点速度，可以先测下速度再决定买哪里的服务器。

- ## 服务器端配置

  ​需要安装的是python版本的`shadowsocks`，因此首先需要安装`python`和安装工具`pip`： 

```bash
Debian / Ubuntu:
$ apt-get install python-pip
CentOS:
$ yum install python-setuptools && easy_install pip
```

​	然后执行安装：

```bash
$ pip install shadowsocks
```

​	安装好后需要修改配置文件，创建/编辑 `vim /etc/shadowsocks.json` ：

```json
{
    "server": "my_server_ip",
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "port_password": {
      "8380": "password0",
      "8381": "password1",
      "8382": "password2",
      "8383": "password3"
    },
    "timeout": 300,
    "method": "aes-256-cfb"
}
```

​	具体各项配置说明：

|     Field     | Explanation                              |
| :---------: | :--------------------------------- |
|    server     | the ip of your server                    |
| local_address | the address your local listens [127.0.0.1] |
|  local_port   | local port                               |
| port_password | password used for encryption             |
|    timeout    | in seconds                               |
|    method     | encryption method, default: "aes-256-cfb" |
|   fast_open   | use TCP_FASTOPEN, true / false           |
|    workers    | number of workers, available on Unix/Linux |

​	配置完后保存，启动Shadowsocks服务：

```bash
start:
$ ssserver -c /etc/shadowsocks.json -d start
stop:
$ ssserver -d stop
restart:
$ ssserver -c /etc/shadowsocks.json -d restart
```

​	如需设置开机自启动，只需把上面的启动命令加到 `vi /etc/rc.local` 里即可。

- ## 客户端配置

  ​客户端直接到 [GitHub](https://github.com/shadowsocks) 上下，安装好后编辑服务器，填上服务器地址和 `/etc/shadowsocks.json` 配置文件里写的对应端口和密码等就能使用了，关于PAC可以直接选择**从GFWList更新本地PAC**，这里的`GWFList`是GitHub上一个开源项目，专门收集惨遭GFW黑手的ip，理论上是最全的PAC文件了。

- ## 加速

  ​[参考](http://wuchong.me/blog/2015/02/02/shadowsocks-install-and-optimize/)