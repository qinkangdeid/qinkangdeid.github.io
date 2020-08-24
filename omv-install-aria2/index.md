# OMV配置Aria2


[TOC]

Aria2是一款非常棒的多线程下载工具，最最关键的是，它支持百度的下载，相信这一定是广大网友的福音

> 下面的步骤需要使用到Docker,如果还没有安装的请先安装，[查看OMV配置Docker](https://www.jianshu.com/p/5b0eacc61527)

这里我们使用docker进行安装aria2，很小，只包含一个daemon进程



- 首先查看你用户的PGID/PUID，

```bash
# 使用 id {your_username} 命令查看即可
qinkangdeid@omv:~$ id qinkangdeid
uid=1000(qinkangdeid) gid=100(users) 组=100(users),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),33(www-data),44(video),46(plugdev),108(netdev),110(ssh),1000(qinkangdeid),997(openmediavault-webgui),996(docker)
```

> 例如我的uid=1000 gid=100(users) 
>
> uid=1000(qinkangdeid) gid=100(users) 就是你想要的了

## 命令行安装

```bash
 docker run \
   -d \
   --name aria2 \  
   -p 6800:6800 \
   -u=1000:100\
   --restart=unless-stopped \
   -v /srv/dev-disk-by-label-kulh2t/appdata/aria2/config:/config \ 
   -v /srv/dev-disk-by-label-kulh2t/downloads:/downloads \
   opengg/aria2
```

容器数据卷地址：

`/config`: aria2配置文件和日志文件地址

`/downloads`： aria2下载存放地址

我计划是将以上两个容器路径映射到我的宿主机硬盘的两个地址上：

> /srv/dev-disk-by-label-kulh2t/appdata/aria2/config : /config
>
> /srv/dev-disk-by-label-kulh2t/downloads:/downloads

你可以映射到你想映射的位置，只有保证文件夹有权限读取即可

命令参数释义：

> -d  : 镜像以后台方式运行
>
> —name : 镜像的名字 可以随便起你觉得容易识别的名称
>
> -p : aria2进程的服务端口 `:`前面是指代宿主机(你安装OMV的实体机器)的端口;`:`后面是容器里的进程端口号
>
> -u: 就是刚才我们获取到的用户的PUID:PGID
>
> -v : 数据卷映射 和-p一样的 前者是宿主机的路径 后者是容器的路径
>
> —restart: Docker容器的重启策略

- 容器的重启策略

Docker容器的重启策略是面向生产环境的一个启动策略，在开发过程中可以忽略该策略。

Docker容器的重启都是由Docker守护进程完成的，因此与守护进程息息相关。

Docker容器的重启策略如下：

`no`，默认策略，在容器退出时不重启容器

`on-failure`，在容器非正常退出时（退出状态非0），才会重启容器

`on-failure:3`，在容器非正常退出时重启容器，最多重启3次

`always`，在容器退出时总是重启容器

`unless-stopped`，在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器

我们将上面的命令复制到命令行窗口执行：

> 记得将两个 —v的路径映射到你的宿主机地址上

等待镜像下载并运行，执行完成后：

使用`docker ps`命令查看容器运行没有

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235400.png)

可以看到，aria2的docker镜像已经在运行了

去查看我们刚才配置的aria2的配置文件路径：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235415.png)

可以看到，当容器运行起来的时候，已经为我们创建了需要的配置文件，这里主要是`aria2.conf`文件,我们查看一下默认的内容：

```bash
save-session=/config/aria2.session
input-file=/config/aria2.session
save-session-interval=60

dir=/downloads

file-allocation=prealloc
disk-cache=128M

enable-rpc=true
rpc-listen-port=6800
rpc-allow-origin-all=true
rpc-listen-all=true

rpc-secret=<password>

auto-file-renaming=false

max-connection-per-server=16
min-split-size=1M
split=16
```

可以看到有一项 其实还没有设具体的值：`rpc-secret`,这是链接aria2服务时需要的token值，把它改成你想要的设置的值即可（随便设置）：例如：rpc-secret=123456

其他的选项可以先展示默认，以后想要改都可以来这里修改



## Web管理界面安装

OMV提供了一个操作界面管理docker镜像，也可以在这里创建拉取运行docker镜像

切换到`服务 ---> docker(容器)`：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235432.png)



如果你刚才使用命令行安装了aria2，那么现在你也可以在把这里看见他的身影了。

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235447.png)

在这里我们也能看到这个容器是在运行的

想要使用Web界面安装aria2，先要搜索到镜像，进行拉取镜像，找到镜像后点击它

![image-20190203133757421](http://upload-images.jianshu.io/upload_images/3647768-b2177667c249003f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



什么也不需要动，直接点击开始，就会开始拉取镜像了...
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235500.png)

输出那里会显示拉取状态，拉取完成后点击`关闭`
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235514.png)

可以看到我们刚刚拉取完成的镜像已经在镜像列表中了

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235530.png)

- 部署运行镜像

选中我们刚刚拉取的镜像，并点击`部署`镜像
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235544.png)

将会弹出一个配置窗口，这个窗口其实就是我们上面命令行的各种 -*的配置！

下面我们按照容器要配置的进行配置，那我怎么知道容器要配置些什么呢？可以去docker-hub上查看该容器需要配置的项目，例如现在的这个容器：[可以去这里查看](https://hub.docker.com/r/opengg/aria2)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235600.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235615.png)

配置完成后，点击保存即可，之后容器会运行起来，直接可以在容器那里查看运行状态

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235630.png)

## 连接管理界面

这个容器是不带GUI界面的，只有一个守护进程在，我们可以使用网上提供的GUI界面连接我们的aria2后台进程

我这里使用的是这个：http://binux.github.io/yaaw/demo/

你也可以找一个别的

打开上面的连接，吧我们的aria2连接上，点击扳手的位置：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235646.png)

aria2的连接连接是：http://IP:6800/jsonrpc

刚才我们还配置了一个token密码，所以我们的连接地址应该写成：

http://token:123456@192.168.50.118:6800/jsonrpc

token: 后面带的就是你刚才设置的rpc-secret=123456的值

之后点击保存,之后如果连接上了，界面右上角就会出现aria2的版本和下载速度标识

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235701.png)


## 百度下载

aria2可以下载百度盘的资源，需要使用一个插件进行转链

地址：https://github.com/acgotaku/BaiduExporter

克隆下来

```bash
git clone https://github.com/acgotaku/BaiduExporter.git
```

然后使用chrome安装上去：
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235715.png)


![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235728.png)

选择你刚才下载插件的地址

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235740.png)


即可看到插件安装完成：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235753.png)


现在随便去找一个百度的下载，就能看见左侧有一个`导出下载`

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235809.png)


设置我们的aria2进程：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235824.png)


把我们刚才连接GUI界面的连接填写上去

http://token:123456@192.168.50.118:6800/jsonrpc

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235840.png)



之后鼠标再次悬浮到`导出下载`，点击选中我们刚刚配置的aria2服务：我的名字改成了OMV，

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235916.png)

之后即可弹出下面的提示

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319235929.png)



之后任务已经在下载了，可以去http://binux.github.io/yaaw/demo/查看你的任务进度
