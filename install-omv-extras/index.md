# OMV安装omv-extras


omv-extras 是omv的第三方扩展插件库，里面有很多的第三方插件可以使用。点击[这里](http://omv-extras.org/joomla/index.php/guides)下载插件安装包，选择你对应的OMV版本,我这里的OMV版本的OMV4。

来到 

`系统` ---> `插件一栏`

## 方法一：GUI安装

- 点击上传

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320121739.png)



- 上传刚才下载的安装包，之后点`是`

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320121753.png)

- 之后点击检查更新，之后会在最后面能找到刚才的安装包插件，在左侧的单选框打上勾，然后点击安装

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320121808.png)

- 弹出安装插件中的弹框，等待安装成功 `关闭按钮`变为可点击时说明安装成功，装完之后记得去`更新管理`查看是否有需要更新的组件，不然会有莫名其妙的情况出现

## 方法二：命令行安装

直接命令行一键安装即可

```bash
# 下载安装包
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install | bash
```

安装完成在左侧菜单树才可看见

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320121503.png)

## 开启插件库

系统 ---> OMV-Extras

点选一个插件，例如dockerCE

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320121517.png)


之后点击启用，保存即可

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320121706.png)



稍等片刻，安装完成，即可看见已启动一栏变绿
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320121721.png)
