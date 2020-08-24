# OMV内网穿透访问



   我们将数据放在了NAS里，目前只能是在家通过局域网才能访问数据，但有时候我们不在家里，出门在外也想能够访问家里的数据呢？你可能想：

- 在工作机上也能访问和同步nextcloud的数据

- 在线查看我昨晚刚挂的电影

- 复制下载nas上的一些文档

.............

所以远程访问数据也显得十分重要。

  nas远程访问的方法有很多种，使用哪种方法区别于你自己家里的网络环境是`有公网IP`还是`没有公网IP`。

如果家里的宽带没有公网IP，那么就需要做内网穿透，找一台能够有外网IP的服务器VPS之类的转一下数据包。内网穿透的服务也有很多很多，例如花生壳、ngrok、frp、natpp等等。

试用了几个工具之后,frp是比较容器配置的，所以我选择了frp来做穿透

## 前期准备

- 搭建了服务的NAS

- 具有公网IP的服务器或VPS

- frp工具（FRP分为服务端和客户端）（[github](https://github.com/fatedier/frp)）

## FRP服务端配置

> 这里需要将FRP服务端部在你拥有公网IP的服务器上，我这里放在了我的VPS上

- 下载FRP(PS:FRP更新好快)

点击[这里](https://github.com/fatedier/frp/releases)选择版本下载

我现在用的版本是：`frp_0.23.1_linux_amd64.tar.gz`

解压命令:`tar -zxvf frp_0.23.1_linux_amd64.tar.gz`

可以看到frp的文件：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320125927.png)

在这里，我们只需要服务端的程序和配置，所以可以删掉客户端的文件，当然 你不删也不会有什么问题，下面配置服务端的配置：

`frps.ini`就是服务端的配置文件，修改它：

```bash

vi frps.ini

```

如下面：

```bash

[common]

bind_port = 7000    #这里是绑定的端口 客户端连接和服务端通信就是用的这个端口

vhost_http_port = 8080  #这里是服务的端口 穿透后访问内网的东西就是通过这个端口在外网访问的

subdomain_host = xxxx.com    #你的域名配置 如果你的公网IP没有绑定域名的话可不用 不填

auth_token=123   #通信验证token  

```

好了，服务端配置就这几行了，完成后启动服务端：

```bash

#cd到你的frp目录，然后启动

cd /opt/frp

# 运行

/opt/frp/frps -c /opt/frp/frps.ini

```

可以看到如下日志：

```bash

2019/02/27 01:40:07 [I] [service.go:124] frps tcp listen on 0.0.0.0:7000

2019/02/27 01:40:07 [I] [service.go:166] http service listen on 0.0.0.0:8080

2019/02/27 01:40:07 [I] [root.go:204] Start frps success

2019/02/27 01:40:07 [I] [service.go:317] client login info: ip [x.x.x.x:24420] version [0.22.0] hostname [] os [linux] arch [amd64]

2019/02/27 01:40:07 [I] [tcp.go:66] [c37655b3b93a54f5] [ssh6007] tcp proxy listen port [6007]

2019/02/27 01:40:07 [I] [control.go:394] [c37655b3b93a54f5] new proxy [ssh6007] success

```

## FRP客户端配置

> FRP的客户端是部在你的内网机器上，也就是你家里的任何一个机器上，可以是路由器或者一台PC或你的NAS上

如上下载frp工具到你的内网机器上，解压，这里我们需要配置客户端，所以`frpc.ini`是我们这次的重点

修改`frpc.ini`文件：

```bash

# frpc.ini客户端配置

[common]

server_addr = x.x.x.x  # 这里填写你刚才配置的服务端的公网IP 公网IP 公网IP 公网IP~

server_port = 7000  #与服务端的7000端口通信

auth_token = 123    #通信验证token

# 下面的就是要穿透的服务了 一个[xxx]开头 算一个服务 多个以此类推

[ssh]

type = tcp    # 协议类型   ssh选这个 

local_ip =x.x.x.x     #这里是你内网中开启服务的机器的IP  如果是本机你可以写127.0.0.1 如果不是查看开着服务的机器的内网IP

local_port = 22    #代理的内网机器的端口

remote_port = xxxx    # 这里是远程的端口 就是服务端所在机器的端口 远程访问就是访问这个端口

[omv]

type = http    # 协议类型   web选这个  我这里是穿透了OMV的WEB管理界面

local_ip = x.x.x.x    #同上描述

local_port = xxxx     #同上描述

```

一个客户端配置就完成了，之后像服务端那样启动客户端

启动完成后查看服务端的日志输出，可以看到客户端已经与服务端通信上了，

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320125945.png)

这样便穿透成功了，之后访问       `公网IP:8080`即可访问到你的omv管理界面了
