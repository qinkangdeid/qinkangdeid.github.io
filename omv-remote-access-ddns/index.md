# OMV-DDNS访问


# 远程访问(公网IP-DDNS方式)

[TOC]

> 宽带有`公网IP`的前提下，使用DDNS的方式相比于内网穿透的方式要更稳定，不需要依赖服务端的带宽和处理能力，速度也可以跑满上行。

zerotier虽说也可以跑满速，但是会有些缺点：

- 连接到了下午时候会比较的慢
- 所有的访问客户端需要安装客户端软件，这是典型的vpn方式，但同时它也更安全一些。



## 前期准备

- 宽带上网IP为公网IP(`最最重要的根源`)
- 光猫改桥接，路由器拨号
- 一个域名
- 一台可装DDNS解析插件的路由器或服务器
  - 路由器有
    - 硬路由的梅林
    - 软路由的openwrt
- 内网在运行的服务


## 获取公网IP


### 查看你的宽带是否是公网IP

- 首先查看你的路由器或光猫上获取到的IP
- 访问[http://www.ip138.com/](http://www.ip138.com/)
- 访问[whatismyip](https://www.whatismyip.com/)

如果上述三个地方的外网IP都是一致的，那么你的宽带就是公网IP的方式


### 尝试获取你宽带的公网IP

这一步是下面所有操作的前提，如果你的宽带没有公网IP，那么你需要联系你的宽带运营商获取，网上也有很多介绍获取的途径方式了，我也搜了挺多的，大概是：联通宽带不好拿，移动就不要想了，电信的就比较容易这样的结论。

不过也稍微注意一下，不要动不动就找客服电话投诉，如果你装宽带的时候你的装机师傅给你留了他的联系方式，你可以先找到装机师傅询问他获取，每个师傅都是有考核的，谁都不喜欢接投诉单，你直接找装机师傅一般都是比较乐意帮忙的，如果他不给或他不能提供的时候，再打`10000`号找人工申诉获取。

下面就是我询问的经过，师傅爽快也干净利落
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131242.png)


## 光猫改桥接

现在装的宽带一般都会默认使用宽带运营商附带的光猫负责拨号上网并兼路由功能（光猫是路由模式），如果要再接一个自己的路由器的话获取到的是一个静态的内网IP（光猫分配的），能够做ddns的设备就不能方便地获取到公网IP。     



 如果你目前也是这种方式上网，那么你也需要修改光猫模式为桥接模式（只负责传输信号），宽带的拨号工作留给路由器，这样路由器直接就可以获取你的公网IP（通过WAN口）了，熟悉你家里光猫的型号，登录后台管理界面，修改光猫模式为桥接模式。


### 改桥接步骤

电信光猫的型号有很多种，但大体都相同，我目前使用的是`TEWA-708G`,以我的为例来操作一下。

- 登陆光猫后台
地址是：[http://192.168.1.1:8080](http://192.168.1.1:8080)
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131301.png)

- 账户密码获取
网上大多文章说一定要拿到超管密码，但是我发现我这个型号不需要，直接使用普通用户进去就可以设置
用户密码就是光猫背面的账户密码了
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131316.png)

- 配置设置向导
只需要配置向导就可以完成桥接配置
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131333.png)
- 选择桥接上网方式
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131347.png)
- 光猫无线网络配置
拨号已经不由光猫来做了，那么无线也不需要它来发射了，关掉它
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131404.png)

- iTV配置
没有iTV
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131419.png)
- 密码修改
按需设置，我这里不修改了
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131432.png)

- 确认配置项目
确认一下刚才进行的配置，如果没有异议，点击完成即可。
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131446.png)
- 查看配置
点击完成后等待或重启光猫再进入后台查看，业务配置，看到上网方式为桥接即为完成
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131503.png)


## PPPoE拨号上网

拨号设备可以是电脑也可以是路由器，这里就和平常操作步骤一摸一样了，没什么要说的，但是一般都会使用路由器拨号吧，毕竟家里上网设备很多。


## 域名申请

免费域名也有很多地方可以注册，不过还是自己买一个吧，阿里云上随便买一个杂七杂八后缀的域名也不花多少钱,几十块就能用好多年，[域名申请](https://wanwang.aliyun.com/domain/?spm=5176.100251.111252.19.74414f157cffn1)

域名申请通过后，创建访问的`accessKey`和`AccessKeySecret`,具体是登录阿里云的控制台，在页面右上角，鼠标悬浮到你的头像，出现一个小浮窗，选择`accesskeys`

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131518.png)

- AccessKey ID 和 AccessKey Secret 推荐使用 子用户AccessKey(访问控制台RAM) 分配的权限，这样权限更少相对来说更安全些
点击会弹出提示，我们还是安全起见，配置一个专门ddns的子用户

点击`开始使用子用户AccessKey`，起个名字
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131534.png)

添加权限
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131549.png)

需要配置的权限：

- AliyunDNSReadOnlyAccess(只读访问云解析(DNS)的权限)
- AliyunDNSFullAccess(管理云解析(DNS)的权限)
依次搜索配置上去
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131605.png)

选择完毕，点击确定
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131619.png)

- 配置accesskey
回到用户管理，点击相应的用户账户进入设置
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131636.png)



点击创建accessKey
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131650.png)

之后要记得`保存`好自己的key和密钥信息，DDNS配置的时候就是使用的这些作为账户密码。
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131708.png)


## DDNS配置

这里演示的都是阿里云的域名，其他域名服务商的配置都大同小异


### 梅林配置DDNS

- 配置ddns
我现在的路由器是刷了小宝梅林的网件R6400，里面有应用市场可以一键安装`aliddns`。下载完成后会在`已安装`列表里
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131728.png)


下面配置解析，按照下面的图示进行配置，中部的`appkey`和`appsecret`就是刚才在阿里云`创建AccessKey`所获取到的两个字符串
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131742.png)

- 端口转发
路由器`外部网络(WAN)` --> `端口转发`，在这里配置你需要转发的端口
例如 我想转发一个内部的http服务，在`基本设置`那里选取 http
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320131758.png)

之后会在`端口转发列表`那里多一条记录，这里需要配置几项：
- `本地IP` ：你搭建的服务的机器所在内网IP 例如：192.168.5.1
- `本地通信端口`：你搭建的服务的访问端口 例如：9999
- `通信端口范围`：这里就是映射到外网访问的端口了
这里的端口就是要映射内网服务端口到外网去
例如： 8999  就是访问的192.168.5.1:9999的内网服务


配置完着三项点击右边的保存即可映射成功。之后你就可以通过上面配置的域名+端口访问这个内网服务了，例如上面的配置 外网访问链接就是： [http://nas.nastemp.abc:8999](http://nas.nastemp.abc:8999)


### Openwrt配置DDNS

- 配置ddns
- 端口转发
