---
title: 利用-AWS-的-EC2来搭建属于自己的-VPN-服务器（MAC平台）
date: 2017-10-11 19:43:36

tags: 
- AWS EC2
- VPN
- 服务器

categories: 其他
comments: true
---

[TOC]
其实网上有不少的教程了，但是我觉得没几篇是真的自己一步一步来的，即使有可能自己摸索出来了，遇到的坑也没说出来，其实还是有很多坑，我是一个追求完美的人。因为最近在利用 AWS 的 EC2来部署自己的 node项目，所以顺手搭建了一个 vpn。
这里假定你已经拥有了 AWS 的账户，没有的先去申请一个，AWS 的服务首年免费，次年如果不想付费可以继续申请一个新的账号
>注意在申请新的账号的时候不要勾选额外的服务，否则你会被无端扣费，我之前就被扣了几百美元，不过国外平台就是服务不错，如果你真没有使用过这些服务，可以申诉回来的，找亚马逊客服，我的五百大洋就找回来了。

-------
<!-- more -->

亚马逊的网站也支持中文简体，英文不好的童靴可以选择中文简体，页面左下方。
![Snip20171214_1.png](http://upload-images.jianshu.io/upload_images/1128807-719c735d069172d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 注：EC2实例就相当于一个轻小型的服务器

## 选择区域
登录到 AWS 首页后，进入 AWS 服务，先选择右上角的地区，这里选择的
地区说简单点就是你搭建的这个服务器所在的区域，我们选择亚太区域东京，可以检测各个区域的网络节点延迟，东京延迟是最低的
![Snip20171214_4.png](http://upload-images.jianshu.io/upload_images/1128807-1de846716eacb2ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 建立 EC2实例
### 选中所有服务->计算->EC2
![Snip20171214_2.png](http://upload-images.jianshu.io/upload_images/1128807-d059a8c267c9269c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 启动实例
点击启动实例
这里不需要管其他东西
![Snip20171214_5.png](http://upload-images.jianshu.io/upload_images/1128807-7074e89876a22cb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>这个截图中可以看到我的账户有两个正在运行的实例，分别绑定了两个 ip 地址，安全组以及秘钥对。后面会介绍


### 选择虚拟机
选择虚拟机，也就是要装在你的服务器的操作系统
勾选仅免费套餐，选中 Ubuntu Server, 点击选择进入下一步
![Snip20171214_6.png](http://upload-images.jianshu.io/upload_images/1128807-78eee5e7c2e06163.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 选择实例类型
这里是默认的，因为只有这个是免费的，所以不需要改动，直接点击审核和启动
![Snip20171214_7.png](http://upload-images.jianshu.io/upload_images/1128807-4d345b1b4fd5dd0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 核查实例启动
然后就到了最后一步，中间的几部不需要管，有兴趣可以点进去看看，但是不需要做任何更改。点击启动
![Snip20171214_8.png](http://upload-images.jianshu.io/upload_images/1128807-476709e164045474.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 创建秘钥
这里需要创建一对秘钥，秘钥是用来远程登录服务器的，也就是接下来会在你自己的电脑上登录 AWS 的 EC2服务器，并在里面配置项目。
- 点击创建新秘钥对
- 填写秘钥名称，可以填 TXVPN
- 下载秘钥对
> 只有下载了秘钥对才能启动实例，一定要保存好这个秘钥对，很重要
- 启动实例
![Snip20171214_11.png](http://upload-images.jianshu.io/upload_images/1128807-8270ecab9c1b6394.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
接下来就可以看到实例已经启动了，这时候可以为实例设置一个名字方便好记
![Snip20171214_13.png](http://upload-images.jianshu.io/upload_images/1128807-c8cf7edb63b822fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 创建安全组
这个安全组可以指定访问的 ip，端口等等。
点击网络与安全->安全组
会看到组名，刚才创建的实例默认是会创建一个安全组的，可以在实例界面拖到最后面查看默认生成的安全组名，然后在这个界面选中与刚才创建的实例相关联的安全组
- 点击入站
- 编辑
![Snip20171214_16.png](http://upload-images.jianshu.io/upload_images/1128807-9bdb9a57adc08c53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 编辑入站规则
- 添加规则
> 选择自定义 TCP 规则，在端口范围中填写1723，任何位置
- 保存
![Snip20171214_17.png](http://upload-images.jianshu.io/upload_images/1128807-fb1e1da46ad64a1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 添加弹性 IP
点击网络与安全->弹性 ip->分配新地址
![Snip20171214_20.png](http://upload-images.jianshu.io/upload_images/1128807-3d96360c12209b8b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 关联弹性 ip
分配完之后 会回到分配新地址界面，这个时候选中已经分配的 ip
点击操作-> 关联地址
![Snip20171214_21.png](http://upload-images.jianshu.io/upload_images/1128807-5ead292e480b6aec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>不添加了弹性ip，你的实例每次重新启动就会改变ip。
> 不能分配与实例没有关联的，否则会额外收费的哦（还是按小时收费，因为我已经走过了这条路，所以千万记得，如果要停止实例也一定要释放掉与它想关联的弹性 ip）！
> ![Snip20171214_45.png](http://upload-images.jianshu.io/upload_images/1128807-37769c5e3bde1641.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>这就是我一不小心没释放掉 ip 需要支付0.04美元，当然关联的请放心，不会收费的

- 选择实例
> 这里会列出所有你创建的实例，所以前面的实例别名很重要，选中别名 TXVPN
- 重新关联
- 关联
![Snip20171214_22.png](http://upload-images.jianshu.io/upload_images/1128807-143f0f04ae095793.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 远程登录服务器，远程连接
点击实例->选择创建的 TXVPN 实例->连接
![Snip20171214_25.png](http://upload-images.jianshu.io/upload_images/1128807-2418d8058102d93f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 打开 mac 的终端或者 iTerm
cd 到你之前下载秘钥的路径
复制点击关联弹出来的弹框上面的两个命令行，如果是一步一步下来的，不需要做任何更改。
![Snip20171214_27.png](http://upload-images.jianshu.io/upload_images/1128807-bf0f2d00746aeb47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第一次连接会出现一个提示，带有（yes/no）字样的，这里输入yes回车即可

如果出现
```ruby
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-1041-aws x86_64)
```
说明已经等了到远程服务器了，只是这个服务器上什么也没有
![Snip20171214_41.png](http://upload-images.jianshu.io/upload_images/1128807-0daf208242fdbbf5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 搭建 VPN 服务器
这里的步骤也是参考网络的文章，但是会备注几点坑
vim 的操作命令也要熟悉一点，至少知道增删改查
### 配置 pptp
1. 在刚才登录的终端输入
```ruby
sudo apt-get install pptpd
```
2. 出现确认后输入 y回车
3. 配置 pptp
在终端输入
```ruby
sudo vi /etc/pptpd.conf
```
找到文件最后面，去掉下面两行的注释符号#，没有的在最后面加上这两行
localip 192.168.0.1
remoteip 192.168.0.234-238,192.168.0.245
4. 按键盘上的ESC键，左下角的– INSERT —字样消失
5. SHIFT键的同时按一下;键，光标跳转到左下角的:号后面，输入 wq 回车
### 添加 DNS 解析
1. 在终端输入
```ruby
sudo vi /etc/ppp/pptpd-options
```
找到带有ms-dns的两行，并修改 dns 为下面谷歌开放的，没有的可以在最后面加上下面两行
ms-dns 8.8.4.4
ms-dns 8.8.8.8
2. 保存并退出 vim（命令操作同上4-5）步
### 添加 VPN 用户
在终端输入
```ruby
sudo vim /etc/ppp/chap-secrets
```
> ！！一定要注意这里的格式 与字段要一一对应

| client | server | secret | IP addresses |
| - | :-: | -: | :-:|
| TXVPN | pptpd| ****** | * |

其中，用户名(client)和密码(secret)可自行设置，服务名必须是pptpd，IP表明允许登录的ip列表，如果允许所有ip可以设置为*。
如果需要给朋友也加以账号，可以在这里填写，注意的是 AWS 免费的每个月都有流量限制的，慎用哦
### 开启IPv4转发
1. 在终端输入
```ruby
sudo vim /etc/sysctl.conf
```
2. 找到
```ruby
#net.ipv4.ip_forward = 1
```
并将注释符号#去掉
3. 退出 vim 编辑器

### 创建NAT规则

1. 在终端输入
```ruby
sudo vim /etc/rc.local
```
2. 在 exit 0 这一行的上面添加一行
```ruby
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE # 将所有目标IP包转向eth0接口
```
3. 退出 vim 编辑器

pptpd默认监听1723端口，所以需要开启相关端口
```ruby
sudo iptables -I INPUT -p tcp --dport 1723 -j ACCEPT
```
> 是--dport哦

### 更新配置
在终端输入
```ruby
sysctl -p
```
### 重启 pptpd 服务
在终端输入并回车
```ruby
sudo /etc/init.d/pptpd restart
```
会出现 ok restarting pptpd：pptpd.servoce
在终端输入并回车
```ruby
sudo netstat -ntpl
```
如果出现
```ruby
tcp 0 0 0.0.0.0:1723 0.0.0.0:* LISTEN 11779/pptpd
```
说明访问正常

## 连接 VPN
在mac 和 iphone 上现在都已经取消了 pptpd 服务，但是可以通过第三方来连接，这里推荐一个软件
`shimo`这个软件有破解版的,附赠下载地址：
[shimo 破解版下载地址](https://www.waitsun.com/shimo-4-1-5-1.html)
打开 shimo
选择 account 添加新账户，选中PPTP/L2TP 协议-> create
![Snip20171214_33.png](http://upload-images.jianshu.io/upload_images/1128807-c317f0bb8de848d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


出现远程连接的弹框
![Snip20171214_34.png](http://upload-images.jianshu.io/upload_images/1128807-b4074d2f593f5e37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 其中RemoteHost填写的是你之前创建的实例的共有 DNS，一定不能填错了
- Username:填写你在步骤`###4.3`配置的 vpnname
- password:填写你在步骤`###4.3`配置的 vpnpassword
然后 create
当你看到
![Snip20171214_35.png](http://upload-images.jianshu.io/upload_images/1128807-df636a425260092f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
说明 VPN 连接成功了
下次只需要打开`shimo`软件点击连接就 OK 了
到此就大功告成了!
![Snip20171214_38.png](http://upload-images.jianshu.io/upload_images/1128807-08893b171ec379db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![https://www.pixiv.net](http://upload-images.jianshu.io/upload_images/1128807-6a3ab3f1e25a78cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![youngjump.jp.com](http://upload-images.jianshu.io/upload_images/1128807-66f6b85f99a1aa55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

