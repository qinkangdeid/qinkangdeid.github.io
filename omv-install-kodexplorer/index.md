# OMV安装可道云(kodexplorer)


[TOC]

> 可道云是一款云端文档管理软件，开源的，基于PHP开发。

为了方便管理，我这里特地新建了一个共享文件夹，专门存放应用的配置文件,我这里选择创建`appdata`文件夹，创建共享文件夹的方式可见[这里](https://www.jianshu.com/p/67b3587bb597)

OMV的磁盘地址一般映射在 `/srv`下面
具体的硬盘信息可以查看 `文件系统`查看

比如我的硬盘

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000217.png)

在我的`/srv`下的地址是：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000232.png)

> 请记得你自己的磁盘位置  后面的命令需要替换成你自己的磁盘地址

所以我的`appdata`文件夹的位置是：`/srv/dev-disk-by-label-kulh2t/appdata`,我将会在这个文件夹下新建一个子文件夹`kodexplorer`来存放可道云的资源文件，你可以选择你愿意存放的地址存放

> 这里记录下我实践后的两种安装方法：
- 使用OMV插件nginx安装
- docker安装

## 方法一：使用OMV插件nginx安装

### 前期准备

下载nginx插件

> nginx属于omv拓展(omv-extras)插件，如果没有安装拓展包是搜索不到的，如果你还没有安装omv-extras的话，请先安装[OMV安装omv-extras](https://www.jianshu.com/p/4a0aa7e48515)

> 系统 —> 插件 —>搜索nginx

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000258.png)

安装完刷新页面即可看到服务

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000315.png)


创建一个Pools

> 这里要注意，我这里选择的pools的用户和用户所在组是`qinkangdeid`和`www-data`，选取的用户一定是要在www-data组下，不然访问不到，这个时候需要将用户加到你指定的pools的组别下面

> 要查看你的用户 属于哪些组 可以使用 `id USER_NAME`查看，例如：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000334.png)


如果你所选的用户还不在你所选的组下，可以将这个用户加入改组中
>  将一个已有用户增加到一个已有用户组中,使用 ` usermod`命令，
>  完整的命令：`usermod -aG group_name user_name`（将`group_name`和`user_name`替换成你的目标组和目标即可。）
>  例如：`usermod -aG www-data qinkangdeid`

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000359.png)

切到`Server`，编辑服务器

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000559.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000614.png)
开启nginx服务

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000628.png)

web服务已经准备好了，下面开始安装应用

### 安装文件管理

```bash
# cd到指定目录 你可以cd到指定你想要安装的位置
cd /srv/dev-disk-by-label-kulh2t/appdata/kodexplorer
# 下载web文件
wget http://static.kodcloud.com/update/download/kodexplorer4.37.zip
# 安装unzip 已经安装忽略
apt-get install unzip
# 解压
unzip kodexplorer4.37.zip
# 赋予权限
chmod -Rf 777 ./*
```

## 方法二：使用Docker安装 

> docker安装的好处是不用管依赖的问题，非常方便也非常容易安装。
> 这里使用的是我根据网上大佬们的Dockfile修改一点东西得来的[镜像](https://cloud.docker.com/repository/docker/qinkangdeid/kodexplorer)
> 数据卷有两个：

- `/var/www/html`: 这个PHP环境静态资源的存放路径，可道云的配置文件后面就是放到了这里，整个docker是一个php环境，所以以后还有静态的资源，都是可以放在这里访问的
- `/data`: 数据映射位置 ，容器里的可道云只能访问容器里的数据，所以要想访问宿主机也就是你NAS机器上的数据， 就需要将宿主机的数据路径映射到容器里面 

- 拉取镜像
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000644.png)
  在`Parameters`处填写要拉取镜像的出处：这里我拉的是：`qinkangdeid/kodexplorer`

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000700.png)

等待一会，拉取完成，即可查看：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000714.png)

下面是运行容器：
选中刚才拉取的镜像，点击`Run Image`：
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000730.png)


配置运行：
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000744.png)


![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000808.png)

之后点击`保存`运行容器，之后可以看到运行的容器：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000828.png)


 > 不想用Docker可视化界面操作的话，也可以使用下面的命令一键安装运行容器，命令执行完成后，在docker可视化界面都能看到容器的状态

复制下面的命令到终端执行
**PS**：**记得替换你的宿主机路径**

```bash
docker run -d -p 9000:80 --name kodexplorer  \
 -v /srv/dev-disk-by-label-kulh2t/appdata/www:/var/www/html  \
 -v /srv:/data  \
 qinkangdeid/kodexplorer
```








## 访问可道云

- 访问路径：http://你的IP:9000/kodexplorer

> 注意刚才设置的9000端口访问的基文件夹是appdata,appdata下面创建了一个`kodexplorer`文件夹存放可道云的资源文件，所以访问可道云的时候需要再加上`kodexplorer`，如果你直接解压在appdata文件夹下面，则不需要加了，为了少开端口，以后还有什么web应用，我都是放在appdata下面的，加一个文件夹区分应用就好了

当你第一次打开的时候，应该看见如下界面，会提示一个错误，
*PS*: docker方式安装不会有这个错误

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000846.png)

```bash
# 那是因为我们没有安装phpGD库环境
error:

须开启php GD库,否则验证码、缩略图使用将不正常
```

解决：

```bash
apt-get install php7.0-gd
```

重新刷新一下界面，看到已经恢复正常

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000902.png)

这里要设置管理员密码，设置你的管理员密码，然后确定，页面自动刷新到新的登录页

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000917.png)

这里无需多言，填写你刚才的用户密码登录吧

登录完之后，安全起见，在用户管理里把其他两个用户禁用/删除（当然，如果你想保留，也是可以的）

> 

删除/禁用多余的用户
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000936.png)

这就是可道云的文件管理界面了

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320000949.png)

> Docker方式安装的注意：运行容器的时候`/data`映射到了宿主机的磁盘位置，所以你要查看宿主机的文件，应当去`/data`下访问

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320001004.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320001022.png)


还有一个看似很吊的桌面，不过我没用过

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320001037.png)


> PS: 在用可道云的时候，你可能发现有些目录只能`只读`而不能写，这是因为OMV的权限设置不允许，可道云的运行用户和组是`www-data`,所以需要把用户和组别加入到共享文件夹中

比如我现在的一个硬盘的`downloads`共享文件夹现在是`只读`状态

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320001053.png)

前往 `访问权限控制管理` --> `共享文件夹`  对共享文件夹进项`ALC`权限控制 如下 将要允许访问的共享文件夹把`www-data`的用户和组别加上访问权限即可，如下图，最后点击`应用`

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320001109.png)

现在刷新可道云的文件管理，可以看到 只读标志 已经消失 ：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320001123.png)
