# Typora设置PicGo图床


[[_TOC_]]

## 安装PicGo

```bash
brew cask install picGo
```


## 配置github图床

仓库：`用户名/仓库地址`

分支: `存放图片的分支`

具体可以直接参考[官方教程](https://picgo.github.io/PicGo-Doc/zh/guide/config.html#github%E5%9B%BE%E5%BA%8A) 我也是去那里扒拉的 这是记录一下 hhhh

- 新建一个仓库

  用于存放图片

  ![img](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/create_new_repo.png)
-  生成授权token

  为你存放图片的仓库添加授权token用于PicGo操作你的仓库
  
  访问：https://github.com/settings/tokens
  
  然后点击`Generate new token`

![img](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/generate_new_token.png)

​		把repo的勾打上即可。然后翻到页面最底部，点击`Generate token`的绿色按钮生成token

![img](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20180508210435.png)
**注意：**这个token生成后只会显示一次！你要把这个token复制一下存到其他地方以备以后要用。

- 配置PicGo

![image-20200823221328042](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/image-20200823221328042.png)

## 安装Typora

```bash
brew cask install typora
```

## 配置Typora

上传服务选择PicGo

![image-20200823221622792](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/image-20200823221622792.png)

验证配置是否成功

![image-20200823221505855](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/image-20200823221505855.png)

还可以设置默认的操作，只有粘贴图片就上传到图床

![image-20200824000737807](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/image-20200824000737807.png)
