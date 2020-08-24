# OMV安装jellyfin


OMV提供了一个Docker的可视化插件
方便用户进行图形化配置docker容器
如果你还没有在OMV开启Docker,请参照[这里](https://www.jianshu.com/p/5b0eacc61527)开启

进行Docker容器的安装一般就几个步骤：
- 去镜像网站确定你要安装的容器的地址([官网镜像网站](https://hub.docker.com/))
- 拉取镜像
- 部署镜像


下面以`jellyfin`个人多媒体管理为例，所有的容器软件安装都是一样的，只是在部署容器的参数有所不同

###  查找镜像
 确定你要安装的镜像的地址，去[官网镜像网站](https://hub.docker.com/)搜索镜像，一般优先查找官方的就行了或最受欢迎的镜像：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124312.png)


镜像网站上的`Overview`你可以稍微看下，因为那里介绍了容器运行的一些可以配置的参数，如果知道看`Dockerfile`的话，也可以知道容器有多少配置项了

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124326.png)

### 拉取镜像

可以看到 `jellyfin/jellyfin`就是官方的源了，我们去OMV的docker-gui界面拉取镜像

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124343.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124359.png)

如上配置后，点击`开始`即可拉取镜像

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124413.png)


![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124430.png)

拉取完成后：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124444.png)

点击右上角的`更新`，即可刷新查看我们拉取的镜像已经在镜像列表中了
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124458.png)


### 部署镜像
最后一步就是部署镜像了，docker提供了`-v`参数可以把容器里的数据卷映射到宿主机(就是我们OMV的机器),当我们安装容器软件时，有一些配置文件或者软件数据我们希望在宿主机上就可以方便的进行编辑和查看，那么我们就需要将它们映射出来，所以最好在你的宿主机上创建一个应用数据的共享文件夹，用来存放各类软件的配置文件和数据，OMV创建共享文件夹的方法可见[这里](https://www.jianshu.com/p/67b3587bb597)
在`docker-gui`界面上选中镜像并点击`部署镜像`
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124515.png)
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124528.png)

例如我配置和容器的端口一样的宿主机端口，一定要点击右边的加号 添加才行！！！！不然不会被保存
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124544.png)
点击右边的加号之后
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124556.png)


环境变量这一栏    可以看容器介绍有没有特殊的环境变量要设置 如果没有就不用设置
但最好配置一下宿主机启动容器所使用的用户的角色ID(PGUD和PUID)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124611.png)

> 首先查看你用户的PGID/PUID 命令行输入 `id 你的用户名`即可出来
```bash
# 使用 id {your_username} 命令查看即可
qinkangdeid@omv:~$ id qinkangdeid
uid=1000(qinkangdeid) gid=100(users) 组=100(users),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),33(www-data),44(video),46(plugdev),108(netdev),110(ssh),1000(qinkangdeid),997(openmediavault-webgui),996(docker)
```
> 例如我的uid=1000 gid=100(users)
- uid对应PUID 
- gid对应PGID 


下面就是数据卷的绑定，这里就是把容器里的数据卷映射到宿主机上的配置，容器可以配什么数据卷一般在其镜像网站上的`Overview`上有介绍，如果没有介绍 你可以在数据卷打上`\`就会有提示这个容器要配置什么数据卷
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124629.png)
可以看到`jellyfin`一般需要配置三个数据卷:
- `/cache`：缓存目录
- `/config`：配置文件目录
- `/media`：多媒体目录

下面是我配置映射的宿主机共享文件夹的路径
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124644.png)
一般其他的配置容器没有特殊说明，就都不用配置了，之后点击`保存`，容器会被运行起来
然后看到`docker-gui`下面的`Docker容器`一栏，点击`更新` 即可看见我们刚才创建的容器的状态了
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124659.png)
`jellyfin`的服务端口是`8096` 宿主机映射出来的也是`8096`
所以我们这个时候可以访问服务，访问服务是访问的宿主机的端口，所以你宿主机映射的端口和容器内的不一样，记得改成宿主机的端口
访问 `http://YOUR_IP:8096`即可访问`jellyfin`的服务了
以下是我完成配置向导后的界面，如果你是第一次访问需要配置一些常规设置
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124717.png)
