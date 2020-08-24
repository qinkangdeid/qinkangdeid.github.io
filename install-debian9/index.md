# 安装Debian9


[TOC]

> 参考链接：https://www.pcsuggest.com/debian-minimal-install-guide/


## 我的硬件
- 主板：华擎J4105-ITX(只支持UEFI)
- 内存：8G
- 系统盘：32G 闪迪酷豆U盘
- 数据盘：2t希捷酷狼




## 前期准备

- debian镜像
- 一个8G以上U盘用于烧录启动U盘
- 安装系统盘（可以是任何闪存介质：U盘、HDD、SSD）

## 烧录启动U盘

安装前需要制作启动U盘，现在Debian的版本是9.8：[下载点我](https://ftp.acc.umu.se/debian-cd/current/amd64/iso-cd/)
安装包在最下面:`debian-9.8.0-amd64-netinst.iso`
烧录软件我使用`Etcher`进行操作，以前一直使用`refus`,觉得已经很简单了，自从找到了`Etcher`,才发现还有更简单的，而且长得也很漂亮，那果断换它了。[点击这里下载](https://www.balena.io/etcher/)

 基本只需要点三次就可以完成：

- 选取烧录镜像

![image-20190202182413643](http://upload-images.jianshu.io/upload_images/3647768-8156df0b83288a27.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 选择要烧录的U盘

![image-20190202214618084](http://upload-images.jianshu.io/upload_images/3647768-7051384b2a3b6d38.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 点击烧录

![image-20190202214634718](http://upload-images.jianshu.io/upload_images/3647768-13c6139d320850d9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后看到烧录进度条

![image-20190202214703619](http://upload-images.jianshu.io/upload_images/3647768-693971dbd1b89763.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

烧录成功，就是这么简单！

![image-20190202214718947](http://upload-images.jianshu.io/upload_images/3647768-d0caa2162df92886.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 安装Debian

> 将烧录好的U盘插到你的机器USB上，开机启动的时候选择U盘启动，之后就可以看到安装向导界面了

1. 选择`图形安装`

![image-20190202215157893](http://upload-images.jianshu.io/upload_images/3647768-3eff291fc67334c2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 选择开机语言

![localechooser_languagelist_0](http://upload-images.jianshu.io/upload_images/3647768-760d53e593bf7a07.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 提示所选的语言可能不能完全显示，继续选择`是`

![localechooser_translation_warn-severe_0](http://upload-images.jianshu.io/upload_images/3647768-fd16801deea95114.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 选择母语区域

![localechooser_shortlist_0](http://upload-images.jianshu.io/upload_images/3647768-cbcdc01572a64889.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



5. 配置键盘，这里选择`美国`或者`汉语`都是可以的，键位都一样

![keyboard-configuration_xkb-keymap_0](http://upload-images.jianshu.io/upload_images/3647768-f6ef873fe2c60966.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6. 等待加载额外组件后，配置网络，主机名可以随意填写，我这里就填写omv

![netcfg_get_hostname_0](http://upload-images.jianshu.io/upload_images/3647768-3a2676ac12b33a93.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

7. 网络域名配置，没什么特殊要求的就先不填写了，后面都可以修改

![netcfg_get_domain_0](http://upload-images.jianshu.io/upload_images/3647768-fb6ace99204b71c6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

8. 设置root用户密码

![passwd_root-password_0](http://upload-images.jianshu.io/upload_images/3647768-91033d363a91bf0d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

9. 增加普通用户，名字当然是可以随选取的，看你的意愿了

![passwd_user-fullname_0](http://upload-images.jianshu.io/upload_images/3647768-7295fcbe3554067b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![passwd_user-fullname_0](http://upload-images.jianshu.io/upload_images/3647768-7a4a01c18a818331.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

10. 设置上面用户的密码

![passwd_user-password_0](http://upload-images.jianshu.io/upload_images/3647768-0598cfd3f7b4290e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

11. 如果机器是UEFI启动的，就会出现下面的提示，我的主板只支持UEFI启动，并且我只会在这一块硬盘上安装一个系统，所以可以选择`强制使用UEFI安装`，如果你一块硬盘已有系统，可以选择否，引导以后也可以修改，`强制使用UEFI安装`保证你这次安装下次开机可以引导到Debian。

![partman-efi_non_efi_system_0](http://upload-images.jianshu.io/upload_images/3647768-26aab0643a8fa3ea.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

12. 磁盘分区,这里我选择`使用整个磁盘并且配置LVM`

![partman-auto_init_automatically_partition_0](http://upload-images.jianshu.io/upload_images/3647768-acbabfd09bc74f4b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

13. 选择你想安装Debian的硬盘盘位

![partman-auto_select_disk_0](http://upload-images.jianshu.io/upload_images/3647768-424a8a6ecd354886.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

14. 磁盘分区，还是按照向导所说，选择所有文件在同一个分区

![partman-auto_choose_recipe_0](http://upload-images.jianshu.io/upload_images/3647768-a5e99c2b6af3e923.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

15. 将分区写入磁盘

![partman-lvm_confirm_0](http://upload-images.jianshu.io/upload_images/3647768-fbc42ab49ae2deea.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

16. 依然是磁盘分区设置，按照如下选择，你不选是，你就到不了下一步啊，哈哈~

![partman-lvm_device_remove_lvm_0](http://upload-images.jianshu.io/upload_images/3647768-4645278e018a24d9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

17. 磁盘分区结束，下面是列出你的分区配置，不满意的还可以往前重新调，如果可以了，就写入磁盘，如下：

![partman_choose_partition_0](http://upload-images.jianshu.io/upload_images/3647768-1ef12206ba56d58f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



18. 最后再次确认一下分区配置，可以选择`是`

![partman_confirm_0](http://upload-images.jianshu.io/upload_images/3647768-aafab2f3837ae8f6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

19. 之后便是安装基本系统的进度条在跑了，稍等片刻…….

![image-20190202222040681](http://upload-images.jianshu.io/upload_images/3647768-c1b016d920c1731a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

20. 安装基本系统完毕后，需要选择包管理镜像地址，选祖国：

![mirror_http_countries_0](http://upload-images.jianshu.io/upload_images/3647768-305fff4c30af8627.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![mirror_http_countries_0](http://upload-images.jianshu.io/upload_images/3647768-832b55cfe4424e7e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

21. 选择镜像地址

![mirror_http_mirror_0](http://upload-images.jianshu.io/upload_images/3647768-ba626780076bf0c0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

22. 包管理代理配置 留空，以后有需要可以在配置

![mirror_http_proxy_0](http://upload-images.jianshu.io/upload_images/3647768-634a2c2217c44062.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

24. 收集用户信息弹框，如果不介意的话，选择是

![popularity-contest_participate_0](http://upload-images.jianshu.io/upload_images/3647768-49e66eb58daed29d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

25. 软件选择了最小安装，那就小到极致点吧，以后想要什么就再加。

> PS: 如果要装OMV,最后这一步不要装图形。

![tasksel_first_0](http://upload-images.jianshu.io/upload_images/3647768-11c0c71c3741a454.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里的安装进度会持续比较长的时间…取决于你的配置和你的网络速度，静下心来喝杯茶等待…….

27. 出现下面的画面，已经大功告成了！，选择继续，机器将重启，这个时候拔出你的启动U盘，之后进入安装好的Debian系统，至此Debian的最小安装就算完了。

![finish-install_reboot_in_progress_0](http://upload-images.jianshu.io/upload_images/3647768-c3c6cfdd4c1eee76.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 安装完后的操作

> 系统安装完成后，会启动到命令行界面，因为我们刚才就是最小安装的，是没有安装图形界面的，这个时候使用你刚才创建的用户名或者你的root用户，输入你的密码，进入到系统中，下面就可以为所欲为了。

```bash
# 切换到root用户
su -           
# 第一步还是先更新
apt-get update

# 安装网络工具包 net-tools ifconfig命令需要
apt-get install net-tools
# vim
apt-get install vim
# curl
apt-get install curl
# 安装sudo
apt-get install sudo
# 替换你的用户名 user_name
usermod -aG sudo user_name
```

## 查看你机器的IP

在安装` net-tools`依赖之后，你可以查看你的机器IP是多少

命令行填写 `ifconfig` 即可查看

![image-20190202232054542](http://upload-images.jianshu.io/upload_images/3647768-400468cdf0c7c3b5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




