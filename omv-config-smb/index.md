# OMV共享文件夹/SMB设置


# OMV共享文件夹/SMB设置

## 挂载硬盘

去看看你的硬盘挂载了没

`存储器 —> 文件系统`

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230458.png)

我这里有两个盘没有挂载，选中它挂载
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230516.png)

全部挂载后

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230532.png)


#### 数据盘挂载位置
> OMV的系统盘和数据盘是分离的，有时候你安装的应用的配置文件夹，就需要保存在你的数据盘内
> OMV的数据盘磁盘地址一般映射在 `/srv`下面
> 具体的硬盘信息可以查看 `文件系统`查看

比如我的硬盘

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230552.png)

在我的`/srv`下的地址是：
    ![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230611.png)

## 磁盘保护

开启磁盘保护，为你所有的磁盘都做这个设置，不用的时候能够休眠一段时间，能一定程度上增加磁盘寿命。

`存储器 —> 磁盘`

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230640.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230657.png)



## S.MART

开启磁盘监控，查看磁盘的健康状态，不过OMV的监测好像有问题，我刚买的酷狼盘，被它一扫出了好多老化磁道，放到DG上扫描有事全绿的，没理由这么快就有问题的，可能这个功能没什么卵用而且还不准，不过还是开启吧

`存储器` —> `S.MART`

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230715.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230732.png)

全部亮绿灯即可

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230747.png)

## 开启共享文件夹

`访问权限管理` —->`共享文件夹`

点击`添加`增加共享文件夹

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230804.png)

名称可以随便填写，设备选择你要共享的硬盘，路径就是你要共享的硬盘上的文件夹了

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230821.png)

保存之后，应用一下配置更新即可

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230847.png)

如果有多个文件夹共享，只需要继续添加即可，步骤都是一样的。

## SMB

开启SMB设置

`服务` —> `SMB/CIFS`

首先先启动服务：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230903.png)

然后切换到`共享`，选择添加

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230921.png)

设置要配置的文件夹，这些文件夹都是在共享文件夹那里配置过的。之后点击保存即可

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319230939.png)
