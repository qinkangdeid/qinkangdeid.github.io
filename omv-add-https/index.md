# OMV-添加https保护


[TOC]

## HTTPS的优点

- HTTPS具有更好的加密性能，避免用户信息泄露；﻿﻿

- HTTPS复杂的传输方式，降低网站被劫持的风险；﻿﻿

- 搜索引擎已经全面支持HTTPS抓取、收录，并且会优先展示HTTPS结果；﻿﻿

- HTTPS绿锁表示可以提升用户对网站信任程度；﻿﻿

- 可以有效防止山寨、镜像网站等




> https相较于http增加了安全性的保障，可以为你的服务增加一道防线，避免被抓包分析。
> 尤其是OMV的安全信息是明文传输的，类似裸奔的感觉，所以有必要弄一弄。



## 前期准备

- 公网IP

- 域名

- DDNS绑定

- ssl证书

- 内网服务

- nginx (反向代理)

## 公网IP

请确认你的宽带网络能够获取到公网IP，不是的先去解决公网IP的问题

##  域名购买

我是在[阿里云](https://www.alibabacloud.com/zh/domain?utm_content=se_1001474823&gclid=Cj0KCQjwn8_mBRCLARIsAKxi0GKpi6idUEjCBbAWwyl01gxRt5mysFBMWWaXAgP4B362qMBthLYb97waAqCCEALw_wcB)购买的域名，也可以选择其他免费域名提供商

##  DDNS绑定

DDNS我在梅林上和LEDE安装的插件，梅林的参考[https://www.jianshu.com/p/ff10e9a44428](https://www.jianshu.com/p/ff10e9a44428)

LEDE的也是差不多的，获取阿里云两个key的教程也在[https://www.jianshu.com/p/ff10e9a44428](https://www.jianshu.com/p/ff10e9a44428)
 配置参照下面

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145025.png)



##  证书申请

- 阿里云证书
买了阿里云的域名后是可以免费申请证书的，可以参考[https://yq.aliyun.com/articles/637307](https://yq.aliyun.com/articles/637307)
- Let's Encrypt
这里是免费的证书申请，有效期只有3个月，但是可以无限续签
  - 路由器插件
    梅林和LEDE有插件
  - 脚本配置[参考这里](https://github.com/ywdblog/certbot-letencrypt-wildcardcertificates-alydns-au)



## 配置访问

### 内网服务

- omv服务
- nextcloud
- 可道云
- 一切web应用

### nginx
omv的web服务用的web服务器就是一个nginx程序，所以不需要再安装nginx了，只需要配置证书和反向代理就行了
omv的nginx配置文件路径是`/etc/nginx/nginx.conf`,我们不修改总的配置文件，只需要在`conf.d`文件夹下新建配置文件即可。
ssh连接到omv机器，`cd /etc/nginx/conf.d`切换到配置文件路径。
使用二级域名的映射，新建一个配置文件例如`vim nextcloud.cof`,将下面的配置添加其中 

```bash
server {
        listen 443 ssl;  # https监听的端口
        server_name nextcloud.abcd.com;  # https 映射的二级域名 这个就是你申请的域名
        ssl on;
        ssl_certificate /etc/nginx/uhttpd.crt;   # 证书路径
        ssl_certificate_key /etc/nginx/uhttpd.key; # 证书路径
        ssl_session_timeout  5m;
        access_log /var/log/nginx/nextcloud.log; #日志路径

        location / {
                proxy_redirect off;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://192.168.100.247:8088; # 反向代理的内网服务IP 端口
        }
}
```
之后`wq!`保存退出。
`nginx -t`检查配置是否有误,成功如下
![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145050.png)


之后`nginx -s reload`使配置生效
之后即可按域名访问内网服务

![](https://raw.githubusercontent.com/qinkangdeid/pics/imgs/20200320145106.png)


 如果还想反代其他的服务，继续使用二级域名的方式的话，下面的配置都是类似的 新建一个server文件配置 只需要修改`server_name`和`proxy_pass`的内容为其他的二级域名和内网服务即可。
