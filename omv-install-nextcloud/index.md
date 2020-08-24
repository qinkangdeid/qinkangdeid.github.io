# OMV配置你的私有云盘--NextCloud


[TOC]

# OMV配置你的私有云盘--NextCloud

现在云盘厂家已经所剩无几，你能选的余地已经很少了，是时候找一个私有的云盘解决方案了。你绝对不是第一个有这个想法的人，所以很幸运，在开源的大世界里，已经有人做了出众的产品（Seafile、OwnCloud，NextCloud等等），NextCloud就是其中一款,它的很多东西，都可以来自云端，这也是它如此受欢迎的原因之一。

目前最新版本是Nextcloud 15（15.0.6 ）。

>  NextCloud 是一款开源网络硬盘系统。任何人都可以自由的获取 NextCloud 程序，在家庭或公司构建私有且免费的网络硬盘。它是完全由你用户控制的私有、安全且功能完整的文件同步与共享解决方案。

> 下面的步骤需要使用到Docker,如果还没有安装的请先安装，[查看OMV配置Docker](https://www.jianshu.com/p/5b0eacc61527)


## 创建nextcloud配置共享文件夹

安装docker容器时候`-v`参数有时候要映射宿主机的磁盘地址
OMV的磁盘地址一般映射在 `/src`下面
具体的硬盘信息可以到 `文件系统`查看

比如我的硬盘

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122422.png)

在我的`/src`下的地址是：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122437.png)

> 请记得你自己的磁盘位置  后面的命令需要替换成你自己的磁盘地址

### nextcloud需要的配置文件夹
omv提倡文件管理操作都能在WEB界面管理，所以这里使用创建共享文件夹和SMB的方式创建

- 先创建共享文件夹
`访问权限管理` -> `共享文件夹`，文件夹尽量见名知意，所以这里就叫`nextcloud`
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122451.png)


进入命令行可以查看创建的文件夹，验证文件夹是否已经创建好
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122507.png)


**PS**: OMV在创建了共享文件夹后，会将共享文件夹映射到一个`/sharedfolders`的文件路径下，所以你有两种访问你刚才创建的共享文件夹的路径，一个是定位到你磁盘的路径，一个是公共的`/sharedfolders`下的路径 这两个路径下的操作都是有效的 你可以随便选一个
比如我刚才创建的nextcloud的共享文件夹，我要找到它，可以去两个路径下都能找到

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122522.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122536.png)

- 设置nextcloud文件夹的SMB共享
> 为了使用SMB用户创建nextcloud的其他配置子文件夹 

前往`服务` -> `SMB/CIFS`, 设置SMB文件夹 方便我们在工作机上操作

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122550.png)

之后在你的工作机上使用SMB协议连接NAS 

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122604.png)

连接上SMB以后，就可以像操作本地文件一样操作NAS文件了，直接鼠标右键点击创建文件夹操作即可创建一个文件夹，
这里我预先准备一下几个子文件夹,用处如下：

- `db` : nextcloud依赖的持久化数据的数据库
- `html`:  nextcloud的资源配置文件夹
- `data`: nextcloud的个人同步文件(你网盘的数据以后就存在这里了)
如下
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122620.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122635.png)





## 数据库准备

 默认情况下，NextCloud使用的是SQLite数据库进行数据存储，它仅适用于没有客户端同步的测试和轻量级单用户设置。当多用户、多设备、大数据量的时候，SQLite就不太合适了，NextCloud支持MySQL，MariaDB，Oracle 11g和PostgreSQL等多种数据库。并且推荐使用MySQL / MariaDB。所以为了一劳永逸，还是用MySQL代替吧，MariaDB是MySQL源代码的一个分支。这里使用MariaDB作为数据库支撑，

- 安装mariadb

  mariadb的安装还是使用docker进行， 打开你的终端，复制以下命令创建mariadb容器，即可。如果不想使用命令行安装，需要使用OMV自带的`docker-gui`界面操作也是可以的，将对应的`-*`的配置填到相应的界面框框内就可以了，类似的操作可以参考[这里](https://www.jianshu.com/p/ff4f3b34f6bc)

```bash
docker run -d --name db_nextcloud \
    -p 3307:3306 \
    -e PUID=1000 \
    -e PGID=100 \
    -e MYSQL_ROOT_PASSWORD=123456 \
    -e MYSQL_DATABASE=nextcloud \
    -e MYSQL_USER=nextcloud \
    -e MYSQL_PASSWORD=123456 \
    --restart=unless-stopped \
    -v /sharedfolders/nextcloud/db:/var/lib/mysql \
     mariadb
```
- 命令参数释义(具体的参数释义可以查看[镜像地址](https://hub.docker.com/_/mariadb))：

> `-p 3307:3306`:  容器服务开放的端口,前者是宿主机的端口，后者是容器内服务的端口
>
> `-e PUID、-e PGID`： 运行容器的用户的权限集id
>
> `-e MYSQL_ROOT_PASSWORD`： 数据库root用户的密码
>
> `-e MYSQL_DATABASE=nextcloud` ：创建一个名称为nextcloud的数据库
>
> `-e MYSQL_USER`：创建一个名称为nextcloud的用户
>
> `-e MYSQL_PASSWORD`：名称为nextcloud的用户的密码
>
> `--restart=unless-stopped`：当容器停止时候重启容器
>
> `-v`：数据卷绑定 前者是宿主机的地址,后者是容器机器的位置
>
>  例如： `-v /sharedfolders/nextcloud/db:/var/lib/mysql` 把容器`/var/lib/mysql`的内容挂在到宿主机` /sharedfolders/nextcloud/db`的位置

以下是我运行的日志

```bash
qinkangdeid@omv:~$ docker run -d --name db_nextcloud \
>     -p 3307:3306 \
>     -e PUID=1000 \
>     -e PGID=100 \
>     -e MYSQL_ROOT_PASSWORD=123456 \
>     -e MYSQL_DATABASE=nextcloud \
>     -e MYSQL_USER=nextcloud \
>     -e MYSQL_PASSWORD=123456 \
>     --restart=unless-stopped \
>     -v /sharedfolders/nextcloud/db:/var/lib/mysql \
>      mariadb
Unable to find image 'mariadb:latest' locally
latest: Pulling from library/mariadb
38e2e6cd5626: Pull complete
705054bc3f5b: Pull complete
c7051e069564: Pull complete
7308e914506c: Pull complete
35e6984cb587: Pull complete
3a173c4702b4: Pull complete
efd003ff8e24: Pull complete
ba5d30791443: Pull complete
f3e943c9e01d: Pull complete
e5243a434e4f: Pull complete
910d8b012ee8: Pull complete
1fb787f18e3d: Pull complete
7a0cfbee5299: Pull complete
6fa7c8911619: Pull complete
Digest: sha256:eb6acf7f599f39c42582e11b1de3866c3934da84cc45190c0aac3e8d046e4053
Status: Downloaded newer image for mariadb:latest
`1be6b4e5f24539e8fc40ca7036a567104b067072044f878a3f4d71104ee8ee9a`
qinkangdeid@omv:~$ 
```

查看docker运行容器，看`NAMES`那一栏下面  有个`db_nextcloud`已经在运行，也能看到我们配置的端口

```bash
qinkangdeid@omv:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
1be6b4e5f245        mariadb             "docker-entrypoint.s…"   14 seconds ago      Up 8 seconds        0.0.0.0:3307->3306/tcp   db_nextcloud
5b9d858e08ab        opengg/aria2        "/init.sh"               24 minutes ago      Up 23 minutes       0.0.0.0:6800->6800/tcp   aria2
```

另一个方式使用`telnet`验证安装是否成功

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122655.png)

## 安装NextCloud

NextCloud的安装也还简单，依然是使用docker，复制下面的命令到终端运行即可 ,记得`-v`的前部分是你宿主机的地址，如果不想使用命令行安装，需要使用OMV自带的`docker-gui`界面操作也是可以的，将对应的`-*`的配置填到相应的界面框框内就可以了，类似的操作可以参考[这里](https://www.jianshu.com/p/ff4f3b34f6bc)

```bash
docker run -d --name nextcloud \
       -p 8088:80 \
       --restart=unless-stopped \
       -v /sharedfolders/nextcloud/html:/var/www/html \
       -v /sharedfolders/nextcloud/data:/var/www/html/data \
       nextcloud
```

- 命令参数释义(具体的参数释义可以查看[镜像地址](https://hub.docker.com/_/nextcloud/))：

> `-p 8088:80`:  容器服务开放的端口,前者是宿主机的端口，后者是容器内服务的端口
>
> `--restart=unless-stopped`：当容器停止时候重启容器
>
> `-v`：数据卷绑定 前者是宿主机的地址,后者是容器机器的位置
>
> Nextcloud安装以及数据库之外的所有数据（文件上载等）都存储在容器地址`/var/www/html`中，要想持久化你的数据，不通过nextCloud也可以查看的话，应当映射到宿主机的某个位置上
>
> Nextcloud的卷配置还是挺多的，例如配置(config)、数据(data)、主题(themes)等
>  - nextcloud的一些卷地址：
> - `/var/www/html` 更新所需的主文件夹
> - `/var/www/html/custom_apps`你自己手动安装的应用位置
> - `/var/www/html/config` 本地配置文件位置
> - `/var/www/html/data` 你的网盘数据存放的位置
> - `/var/www/html/themes/<YOU_CUSTOM_THEME>` 主题文件位置
>
> 以上卷映射我这里只把data单独抽出来了，其他的配置全部默认放在`/var/www/html`映射的位置里

下面是我的运行日志

```bash
qinkangdeid@omv:~$ docker run -d --name nextcloud \
>        -p 8088:80 \
>        --restart=unless-stopped \
>        -v /sharedfolders/nextcloud/html:/var/www/html \
>        -v /sharedfolders/nextcloud/data:/var/www/html/data \
>        nextcloud
Unable to find image 'nextcloud:latest' locally
latest: Pulling from library/nextcloud
5e6ec7f28fb7: Pull complete
cf165947b5b7: Pull complete
7bd37682846d: Extracting [===========================================>       ]  58.49MB/67.44MB
7bd37682846d: Pull complete
99daf8e838e1: Pull complete
ae320713efba: Pull complete
ebcb99c48d8c: Pull complete
9867e71b4ab6: Pull complete
936eb418164a: Pull complete
5d9617dfb66b: Pull complete
8dd7afaae109: Pull complete
8f207844da7e: Pull complete
adb3ae5e4987: Pull complete
44d7d07029db: Pull complete
fb91064652b0: Pull complete
50923e16d552: Pull complete
a7cb9c70c5d2: Pull complete
728e578e40fa: Pull complete
4c3163d09df1: Pull complete
842c4700643d: Downloading [===========================================>       ]  35.98MB/41.57MB
842c4700643d: Pull complete
cc1964f4bb3e: Pull complete
125e01596295: Pull complete
Digest: sha256:a2f2bd57fcfd92b3b6c23b6f34f65d59c9b25e7cc883b1ac67fff01229325692
Status: Downloaded newer image for nextcloud:latest
b2b8cb3a61967ba08cb64490c6c8d2a173882560ae22fb0a6a45f895dea36912
```

例行惯例，查看容器运行：

```bash
qinkangdeid@omv:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
b2b8cb3a6196        nextcloud           "/entrypoint.sh apac…"   42 seconds ago      Up 38 seconds       0.0.0.0:8088->80/tcp     nextcloud
cb81622828c1        mariadb             "docker-entrypoint.s…"   7 minutes ago       Up 5 minutes        0.0.0.0:3307->3306/tcp   db_nextcloud
5b9d858e08ab        opengg/aria2        "/init.sh"               About an hour ago   Up 5 minutes        0.0.0.0:6800->6800/tcp   aria2
```

## 访问NextCloud

我把nextcloud的服务端口映射到了宿主机的8088端口上，nextcloud启动后，浏览器输入`http://你的IP:8088`即可访问nextcloud了。

刚开始的界面如下：

- 第一项配置是配置日后访问nextcloud的一个管理员用户名和密码

- 第二项是数据目录，这个我们在运行容器的时候已经指定了位置，所以这里不用动

- 第三项就是配置外置数据库的连接信息了，将之前我们创建`mariadb`的信息填写进去，其实这里也可以在安装nextcloud的时候将之前的mariadb容器连接进去，这里就手动填写了。要注意数据库的服务端口如果宿主机和容器端口映射的不一样的话，这里要写宿主机的端口，例如现在我的mariadb容器的服务端口3306映射到了宿主机的3307端口上了，所以这里使用的端口为3307。

所有信息填写完成后，点击安装完成，这一步骤会比较的耗时间，nextcloud需要创建数据库表和一些初始化配置信息


![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122713.png)

点击安装完成
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122730.png)
安装完成后，会自动显示以下界面

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122747.png)

里面会有一些默认的示例文件

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122805.png)

至此nextcloudu已经安装完成，你可以继续探索NextCloud所拥有的功能。




## 拓展安装方式 

> PS: 上面的安装方式需要在第一次启动访问nextcloud的时候配置数据库连接、数据卷地址，下面的方式可以减少这个步骤
> 也就是可以减少下面这张图的配置情况：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122821.png)

> docker可以使用 --link 连接另外一个容器的配置  nextcloud也支持`通过环境变量自动配置`
> 可以在首次运行nextcloud容器时预先配置安装页面上询问的所有内容。要启用自动配置，
> 可以通过以下环境变量设置数据库连接。但是只能使用一种数据库类型！

Nextcloud目前所支持的环境变量自动配置如下：

- 配置数据库的环境变量
    - 针对选用SQLITE_DATABASE数据库的参数:
      - SQLITE_DATABASE:  使用SQLite数据库时候的数据库名称(我们不适用这个数据库所以不用带这个参数)

  - 针对选用MYSQL/MariaDB数据库的参数:

    - `MYSQL_DATABASE`: 要使用的数据库的名称 在这里和之前创建的`mariadb容器` 的数据库一直`db_nextcloud`
    - `MYSQL_USER`:数据库用户名
    - `MYSQL_PASSWORD` 数据库用户名密码
    - `MYSQL_HOST` : 配置的数据库容器的名称`docker run -d --name db_nextcloud `的`--name`参数指定的名称，也就是`db_nextcloud`

  - 针对选用PostgreSQL数据库的参数:

    - `POSTGRES_DB` :postgres数据库的名称 
    - `POSTGRES_USER` :postgres数据库用户名
    - `POSTGRES_PASSWORD` postgres数据库用户的密码(对应上面用户名的密码)
    -  `POSTGRES_HOST`:配置的数据库容器的名称

- 配置Nextcloud 管理员用户密码的环境变量
    - `NEXTCLOUD_ADMIN_USER` :管理员用户名
    - `NEXTCLOUD_ADMIN_PASSWORD`:管理员密码

- 配置Nextcloud 数据文件地址和表名前缀的环境变量
  - `NEXTCLOUD_DATA_DIR` : nextcloud数据存放的位置(默认路径是: `/var/www/html/data`) 这个不设我们还可以使用`-v`参数来映射
  - `NEXTCLOUD_TABLE_PREFIX`: nextcloud依赖的数据库表表名前缀 (默认是: *""*) 可选操作 一般也不会设啦

所以创建`nextcloud容器`的方式就稍稍有些变化了，可以根据上面的环境变量自由添加，环境变量使用`-e`追加，我这里只配置数据库的环境变量，如下命令所示：
```bash
docker run -d --name nextcloud \
       --link db_nextcloud:db_nextcloud \
       -p 8088:80 \
       -p 4433:443 \
       -e PUID=1000 \
       -e PGID=100 \
       -v /sharedfolders/nextcloud/html:/var/www/html \
       -v /sharedfolders/nextcloud/data:/var/www/html/data \
       -e MYSQL_DATABASE=nextcloud \
       -e MYSQL_USER=nextcloud \
       -e MYSQL_PASSWORD=123456 \
       -e MYSQL_HOST=db_nextcloud \
       --restart=unless-stopped \
       nextcloud
```

我的执行日志：

```bash
root@omv:/sharedfolders/nextcloud# docker run -d --name nextcloud \
>        --link db_nextcloud:db_nextcloud \
>        -p 8088:80 \
>        -e PUID=1000 \
>        -e PGID=100 \
>        -v /sharedfolders/nextcloud/html:/var/www/html \
>        -v /sharedfolders/nextcloud/data:/var/www/html/data \
>        -e MYSQL_DATABASE=nextcloud \
>        -e MYSQL_USER=nextcloud \
>        -e MYSQL_PASSWORD=123456 \
>        -e MYSQL_HOST=db_nextcloud \
>        --restart=unless-stopped \
>        nextcloud
Unable to find image 'nextcloud:latest' locally
latest: Pulling from library/nextcloud
27833a3ba0a5: Already exists
2d79f6773a3c: Already exists
f5dd9a448b82: Already exists
95719e57e42b: Already exists
cc75e951030f: Already exists
78873f480bce: Already exists
1b14116a29a2: Already exists
887fc426d9b4: Pull complete
e8a2a7e68e47: Pull complete
44116bd4b499: Pull complete
5a7ed133cf7c: Pull complete
a0cc2e7ce3b9: Pull complete
3ea943f2a6e6: Pull complete
dc6fe404fa96: Pull complete
2970a87ebdd8: Pull complete
632923a6d419: Pull complete
78f88b7ec6fe: Pull complete
a62deb12226c: Pull complete
30d2885ecc94: Pull complete
5c72c2211abe: Pull complete
Digest: sha256:e4c59de7d564a7cec680d32ebed64bb7a7c53859d1c9dd6ef21912183719b203
Status: Downloaded newer image for nextcloud:latest
8aa333d93a38589c758efd44db429b28e65301205516d940c5e772212ce09b77
root@omv:/sharedfolders/nextcloud#

```

之后nextcloud启动，第一次访问nextcloud就出现如下画面：
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122841.png)

数据库和数据卷的配置没了，只需要创建一个管理员账户即可
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320122858.png)
