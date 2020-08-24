# Debian9安装OMV4


> **OMV5已经推出，原生支持UEFI启动。可以尝试直接使用官方ISO安装了。**
>  [点我前往镜像下载地址](https://sourceforge.net/projects/openmediavault/files/)



​         家里有些硬件留着不使用有些可惜，放着也是放着，不如发挥一点作用,差块主板就是PC，所以就买了块主板，也借此机会学习一下如何组建nas，在nas系统上的选择，最后选择了开源的OMV，OMV基于Debian开发，我的主板只支持UEFI启动，OMV4版本目前不能直接UEFI引导，所以需要使用到Debian，这里做个记录，[查看最小安装Debian](https://www.jianshu.com/p/d7584761d44f)

## 软件版本
- Debian9
- omv4

## 安装前操作

Debian上安装OMV，需要提前解决一些依赖问题

- Install Realtek Firmware


> **不先安装的话之后安装OMV会报下面的错：**

```bash
Possible missing firmware /lib/firmware/rtl_nic/rtl8168d-1.fw for module r8169
```

添加下面内容：
 ```bash
 # 增加仓库 
vim /etc/apt/sources.list
# 在显示界面 按下字母 i  进入插入模式  复制下面的字符串添加到 sources.list 的最后一行   
# 之后 输入  :wq! 保存退出 
deb http://httpredir.debian.org/debian/ stretch main contrib non-free
 ```

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319195554.png)

之后还是在命令行操作：
```bash
# 更新 
sudo apt update
# 这一步 安装 Realtek Firmware
sudo apt-get install firmware-realtek
sudo apt update
# 安装其他内核 
apt-get install firmware-linux firmware-linux-free firmware-linux-nonfree
```


## 安装OMV


- 添加包存储库

  复制下面命令到命令行回车执行

```bash
cat <<EOF >> /etc/apt/sources.list.d/openmediavault.list
deb http://packages.openmediavault.org/public arrakis main
# deb http://downloads.sourceforge.net/project/openmediavault/packages arrakis main
## Uncomment the following line to add software from the proposed repository.
# deb http://packages.openmediavault.org/public arrakis-proposed main
# deb http://downloads.sourceforge.net/project/openmediavault/packages arrakis-proposed main
## This software is not part of OpenMediaVault, but is offered by third-party
## developers as a service to OpenMediaVault users.
# deb http://packages.openmediavault.org/public arrakis partner
# deb http://downloads.sourceforge.net/project/openmediavault/packages arrakis partner
EOF
```

- 安装openmediavault 4（Arrakis）软件包

  这块也是一样 复制下面命令到命令行回车执行

```bash
# 准备工作
export LANG=C
export DEBIAN_FRONTEND=noninteractive
export APT_LISTCHANGES_FRONTEND=none
apt-get update
apt-get --allow-unauthenticated install openmediavault-keyring
apt-get update

# 下面是正式安装
apt-get --yes --auto-remove --show-upgraded \
    --allow-downgrades --allow-change-held-packages \
    --no-install-recommends \
    --option Dpkg::Options::="--force-confdef" \
    --option DPkg::Options::="--force-confold" \
    install postfix openmediavault
```
命令行安装会一直在刷安装日志，等待日志刷完，会跳出如下安装成功界面，这个时候表示安装完毕，选择确定之后回车，再按照要求初始化OMV即可。

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319195704.png)

- 初始化omv

这一步骤就相当简单了，只需要一条命令

```bash
omv-initsystem
```

等待初始化完成后，OMV已经启动完成，之后便可以在浏览器中访问你的刚刚安装的OMV了。

- 修改服务端口

OMV的默认端口的80，但是一般还是不要使用80端口

命令行输入：

```bash
omv-firstaid  # 这是OMV的命令行界面配置面板
```

我们选择第二项(`configure web control panel`)修改Web的访问端口，当然这一个操作是可以在Web界面修改的

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319195841.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319195907.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319195936.png)

按确认后即可成功修改。

```bash
Updating web control panel settings. Please wait ...
The web control panel settings were successfully changed.

The web control panel is reachable via URL:
enp3s0: http://192.168.50.118:9529
enp3s0: http://fe80::7285:c2ff:feaf:f988%enp3s0:9529
root@omv:/home/qinkangdeid#
```

验证端口是否修改成功

```bash
curl http://127.0.0.1:9529
## 如果返回一个html即表示已经成功

# 或Telnet
telnet 127.0.0.1 9529
##  出现下面即可表示端口监听成功
qinkangdeid@omv:~$ telnet 127.0.0.1 9529
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
```



### 登录OMV

如上设置后，OMV便可以访问了，OMV的访问地址是http://YOUR_IP:YOUR_PORT

如果你没有修改OMV的访问端口的话，那么直需要在你的浏览器上打上你的OMV机器所在的内网IP即可

如果你修改了访问端口，就需要在后面加上端口，

例如我的修改成了9529

我的访问地址则是：http://192.168.1.118:9529 ，OMV的登录界面如下：

> 如果你现在还不知道你的OMV所在机器的内网IP，可以使用 ifocnfig 命令查看（需要事先安装net-tools）

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319200123.png)

OMV初次使用的默认密码是以下，前面最后的安装提示也有显示的:

> admin
> openmediavault  

进入后的界面大致如下：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319200154.png)

进去第一件事当然是修改默认的密码

> 系统 ---> 常规设置 --->  web管理员密码

填入你的新密码，之后注销使用你修改后的用户密码重新登录。

## 安装后

### 更新

安装后设置密码后的第一件事件就是先去检查更新，

> 系统 ---> 更新管理   --->  检查更新

如果有更新，软件包信息下面就会显示一长串软件信息，之后点击`升级`按钮即可

如果没有，软件包信息下面会什么都没有，例如我现在的是最新，目前还没有任何资源需要更新

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319200244.png)


![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319200328.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319200352.png)



### 设置非root用户连接ssh(可选操作)

最好不要使用root做太多设置，如果sudoer能做的事情，就让sudoer做，现在为我们的Debian用户添加进ssh组，允许使用改用户进行ssh连接

> 访问权限管理 ---> 用户

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319200427.png)



选择`编辑`切到`用户组`找到`ssh`组并且打上勾，之后保存，之后即可使用该用户进行ssh连接

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319200454.png)
