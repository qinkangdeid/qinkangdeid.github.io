# OMV配置Docker


docker属于omv拓展(omv-extras)插件，如果没有安装拓展包是搜索不到的，如果你还没有安装omv-extras的话，请先安装[OMV安装omv-extras](https://www.jianshu.com/p/4a0aa7e48515)
Docker真是一个强大的支持，后面要用的很多软件都会优先选择docker的版本

## 启用Docker服务

找到`系统` —> `OMV-Extras`开启`DockerCE`库

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231616.png)

之后点击启用，保存即可

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231630.png)

稍等片刻，安装完成，即可看见已启动一栏变绿

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231645.png)

至此，Docker环境已经创建好了。

## 安装docker-gui插件

> OMV还额外提供一个GUI界面管理docker镜像

一样的找到`系统 ---> 插件`如往常一样安装上即可，这个插件安装可能有点久

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231701.png)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231715.png)

之后在服务下就会多一个Docker标签

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231732.png)

- 开启Docker-GUI

  docker镜像的默认位置是`/var/lib/docker`下，如果你想修改默认位置，那就选择第二红框，将位置修改，这里我创建了一个docker-base文件夹
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231745.png)

- 安装完docker之后报错解决
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231759.png)

> error message: Failed to execute XPath query '/config/services/docker'.

输入以下命令解决[参考](https://forum.openmediavault.org/index.php/Thread/21119-error-message-Failed-to-execute-XPath-query-config-services-docker/)

```bash
# 
apt-get purge openmediavault-docker-gui
# 
apt-get install openmediavault-docker-gui
```
## 将GUI用户添加到docker组
   docker安装好后，默认只有root用户可以启用，如果你想非root启用docker的话，就需要将用户添加到docker组
  - WEB界面添加

切换到`访问权限管理--->用户`

选中你要添加进组的用户，点击编辑

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231819.png)



在弹出的窗口再切换到用户组的标签，将docker勾选上

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231833.png)

使用你的非root用户就可以使用docker的命令了：

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200319231848.png)
