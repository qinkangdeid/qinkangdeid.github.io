# Rock64安装OpenMediaVault


血来潮就买了一块rock64, 大小就信用卡大小

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145554.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145610.png)

还可以刷挺多ROM的，这货有千兆网口，关键还有一个USB3口，比树莓派底子好，拿来做简易NAS也是不错的选择

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145626.png)


![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145641.png)

## 前期准备

- rock64
- TF内存卡
- 读卡器

## 安装OMV

- 下载镜像
 rock64有专门制作的OMV镜像：[点我直达](https://github.com/ayufan-rock64/linux-build/releases/)，两个镜像都差不多，如果是大内存的就选64位的arm64,我买的4G的，所以我选`stretch-openmediavault-rock64-0.7.11-1075-arm64.img.xz`  
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145658.png)

- 制作镜像

  烧录软件使用[Etcher](https://www.balena.io/etcher/)，三步搞定 很容易

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145717.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145732.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145746.png)

之后，讲TF卡插入rock64卡槽位置，通电，启动即可。

## 登陆OMV

刚刚启动会刷一两分钟日志，之后出现登录界面：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145802.png)

用你熟悉的任何方式获取到rock64的IP地址，例如登录终端，查看路由器连接等

rock64的终端用户密码默认为：

> - 用户名: root
> - 密码: openmediavault



获取rock64的IP地址后访问OMV的GUI界面

http://YOUR_IP

OMV管理界面的默认用户名密码为：

> - 用户: admin
> -   密码: openmediavault

Rock64 的固件已经帮助安装了`omv-extras`、`flashmemory`,还是很贴心的。



SSH远程登录话要记得在SSH界面勾选开启

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145820.png)

其他配置和X86的无异
