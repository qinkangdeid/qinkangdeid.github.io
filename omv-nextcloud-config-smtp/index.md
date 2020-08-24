# Nextcloud配置SMTP邮件通知


# Nextcloud配置SMTP邮件通知

> 参考地址：https://www.techrepublic.com/article/how-to-configure-smtp-for-nextcloud/
> 官方文档说明：https://docs.nextcloud.com/server/15/admin_manual/configuration_server/email_configuration.html

在Nextcloud的设置选项里有一个邮件服务配置(`设置` --> `基本设置`  --> `电子邮件服务器`)，它能够提供以下功能：
  - 发送密码重置电子邮件
  - 通知用户新文件共享
  - 文件更改和活动通知

以上的功能需要使用到邮件服务器，不过Nextcloud并没有提供可用的SMTP服务器，这个就需要用户们自己配置适用于自己的邮件服务器。我这里使用的是Gmail SMTP服务器。
## 前期工作
- 正在运行的Nextcloud实例(这个必须是有的)
- 使用具有修改电子邮件服务的用户进行登录nextcloud
- 一个Gmail帐户
- 创建一个Google帐户应用密码

创建一个Google账户应用密码[点击这里进入](https://myaccount.google.com/apppasswords)

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320123954.png)

接下来输入一个应用名称方便记忆：`nextcloud`，之后点击生成

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124020.png)

之后会弹出一个弹窗出现应用密码，复制黄色框框里的这个密码备用

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124052.png)


### 配置nextcloud的邮件服务器
  以`管理员用户身份`登录Nextcloud服务器。单击右上角的个人资料图片，然后从下拉列表中单击`设置`。在`管理`(在左边的导航栏中)下，单击`基本设置`,在这里你就能看到 `电子邮件服务器`的表单，Nextcloud的SMTP服务器设置，你需要配置以下内容：
> 发送模式：SMTP
> 加密：SSL / TLS
> From address：您将使用的GMail地址。
> 验证方法：登录
> 需要验证：启用
> 服务器地址：smtp.gmail.com
> 端口：465
> 凭据：用户名/应用程序密码


![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124110.png)


配置完所有内容后，单击`保存凭据`按钮，然后单击`发送电子邮件`按钮。按照正常预期的话，你现在可以去检查您的电子邮件帐户以查看是否收到测试电子邮件。如果收到测试邮件，则说明配置正确完成了。
收到的测试邮件如下：
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320124125.png)

以上就是为Nextcloud配置SMTP的全部内容。如果需要使用其他SMTP服务器，您只需简单的改下配置就可以了。网易邮箱可以参考[这里](https://blog.csdn.net/qq_30754565/article/details/81610531)
