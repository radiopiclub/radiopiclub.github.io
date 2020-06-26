---
layout: post
title: headless
---



# 无头(Headless)操作RadioPi



所谓“无头”，就是不给RadioPi准备显示器、键盘和鼠标，而是用另外一台电脑或者手机，通过网络来操作它。

为什么要无头操作？理由很简单：

- 野外通联时，力求极致的便携
- 远程控制时，咱也用不上家里的显示器啊……



![一套RadioPi和FT-818的便携集成](./img/ft818_gopack.jpg)



为简化表达，接下来我们把运行RadioPi的树莓派电脑简称为RadioPi。





## VNC，无头操作的关键工具



VNC是Virtual Network Console的缩写，虚拟网络控制台。简言之，它类似QQ的“远程协助”，能把RadioPi“投射”到另外一台电脑上或手机上，实现远程操作。



![VNC的基本概念，取自realvnc.com](img/server-modes-service.png)



我们在RadioPi上配置好了VNC Server以及相关的服务。接下来还需要你在另外一台电脑或者手机上，安装VNC Viewer。



### VNC Viewer下载地址：

- [https://www.realvnc.com/en/connect/download/viewer/](https://www.realvnc.com/en/connect/download/viewer/)
- 对于安卓手机用户，国内的应用商店可能没有。推荐从RealVNC公司直接下载[.apk安装包](https://help.realvnc.com/hc/en-us/articles/360002762697)。



## 给无头RadioPi连上局域网

第一次启动RadioPi时，推荐把它连上家里的局域网。通常你家的Wi-Fi网络就是一个局域网，所有的电脑、手机、RadioPi，都在这个网上。

![局域网里的VNC连接 取自realvnc.com](img/get-connected-local-3.png)



让RadioPi联网很简单：**用网线把RadioPi和Wi-Fi路由器连起来。**



### 在局域网里找到RadioPi

#### 在路由器上找radiopi

找到RadioPi的最简单方案，是到路由器上看看分给它的IP地址是什么。RadioPi的默认主机名是小写的radiopi。找到它对应的IP。

![在路由器上找到radiopi对应的IP地址](img/router_ip.png)

然后打开VNC Viewer，在地址栏直接输入这个IP地址，即可访问到RadioPi。

![在VNC Viewer地址栏直接输入IP地址](img/vnc-ip-address.png)



#### 在局域网里扫描IP地址

如果你无法登录路由器，还可以用IP扫描工具在局域网里扫出radiopi的IP地址。这里推荐的软件是：IP Angry Scanner，它把你的电脑所在的局域网整个扫一遍，列出来结果。

![在Angry IP Scanner中，扫到radiopi主机的IP地址](img/angryip-scan.png)

它扫到了RadioPi的IP地址是192.168.32.209。

Angry IP Scanner的下载地址：
https://angryip.org/download/



### 在支持mDNS的设备上直接访问radiopi.local

在苹果的电脑、手机、iPad上，打开VNC Viewer，在地址栏中直接输入radiopi.local，即可访问到RadioPi。



![在VNC Viewer地址栏直接输入radiopi.local](img/vnc-link-local.png)

| **RadioPi上的自动广播主机名服务**                            |
| ------------------------------------------------------------ |
| <sup>RadioPi的默认主机名是小写的radiopi。我们在RadioPi上配置了支持mDNS(多播域名)协议的Avahi服务。简言之，当RadioPi联网后，Avahi就在局域网内广播它的主机名，方便你找到它。它的网址就是：radiopi.local。 <br/><br/>**在苹果电脑或手机上使用Bonjour服务**<br />在苹果公司的设备上有Bonjour服务，它和Avahi一样支持mDNS协议。所以苹果的设备都可以直接访问radiopi.local。 <br /><br/>**在Windows上使用Bonjour服务**<br />微软的Windows系统中，默认是没有Avahi或者Bonjour服务的。有一个简单的方法：安装苹果公司的Bonjour打印服务程序，即可使Windows拥有类似的功能。Bonjour打印服务Windows版的下载地址：https://support.apple.com/kb/DL999 <br /><br />**在linux系统上使用avahi服务**<br />在各种Linux类的电脑上，和树莓派一样要安装支持mDNS的服务avahi-daemon，过程这里不再赘述。<br /><br/>**在安卓手机上……**<br />安卓手机不在用户的操作层面提供mDNS服务。所以需要在路由器上查找，或者扫描局域网IP的方式，获取到radiopi的IP地址。 此外，RealVNC提供一种“云连接”的方式访问RadioPi。在完成设置之后，可以简化查找IP地址的过程。稍后会介绍。 </sup>|





第一次与RadioPi建立连接时，VNC Viewer会有一个安全提示，选择Continue继续。

![第一次连接VNC的安全提示](img/vnc-checkid.png)

接下来VNC Viewer会提示你输入登录radiopi.local的用户名和密码。我们用RadioPi的默认用户pi登录。

- 用户名：pi
- 密码：radiopi599

![输入登录RadioPi的用户名密码](img/vnc-login.png)

然后你就会在VNC Viewer的窗口里看到RadioPi的桌面。你可以像操作普通的电脑一样操作它了。至此，你已经实现了局域网内的RadioPi远程控制。

![第一次登录到RadioPi的桌面](img/vnc-logined.png)

现在你可以通过VNC Viewer给RadioPi添加Wi-Fi无线网络，甩掉网线吧。





## 从公园遥控家里的RadioPi

如果想实现更远的(跨互联网)远程控制——从公园里控制家里的树莓派——那就需要注册RealVNC提供**“云连接(Cloud Connect)”**服务。

![通过互联网访问家里的VNC Server 取自realvnc.com](img/get-connected-internet-1.png)


你需要在去公园之前，要完成以下操作：

- 访问网站 https://manage.realvnc.com 注册一个RealVNC账号。
- 在RadioPi的VNC Server上登录这个账号。
- 在手机的VNC Viewer上也登录这个账号。



每台设备第一次登录时，RealVNC都会要求用电子邮件进行安全验证。验证完之后，远程的RadioPi就会出自动现在VNC Viewer的地址簿中，下次访问时直接点击它就好。

![建立云连接之后的VNC Viewer窗口](img/vnc-teambook.png)



## 高级攻略：野外操作RadioPi

终于说到了野外通联。无头的RadioPi和手机或者Pad配合起来，可以完全取代笔记本电脑。

我们仍然要依赖VNC提供的服务，这意味着还要让RadioPi联网。野外最佳的联网手段，就是用手机共享一个Wi-Fi热点。

所以我们在RadioPi里预先保存了一个叫“radiopi”的无线网络连接：
- ssid: radiopi
- 密码: radiopi599
- 加密方式：WPA-PSK

只要你在手机上共享一个叫“radiopi”的热点，把密码设置为：radiopi599，RadioPi开机就会自动连接上来。

在iPhone、iPad上，共享热点的名称就是它的主机名。需要在“设置/通用/关于本机”中，把名称改成radiopi。

RadioPi联网之后，iPhone可以直接用VNC Viewer访问radiopi.local，安卓手机用户可以访问“云连接”地址簿里的RadioPi。

## 硬核攻略：手动给RadioPi添加Wi-Fi

更技术性的方式，是直接在RadioPi的SD卡上放入Wi-Fi配置文件：
文件名：wpa_supplicant.conf
格式：txt文本

在此文件中，你可以写入多个Wi-Fi的连接信息。首先，用电脑上的“记事本”这样的工具建立一个名叫wpa_supplicant.conf的文本文件。然后把以下这段代码贴进去，并且修改为你自己的Wi-Fi热点信息。ssid就是热点的名字，psk是密码。以下是配置文件的内容：

```shell
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
ssid="radiopi"
psk="radiopi599"
}

network={
ssid="家里Wi-Fi热点的名字"
psk="Wi-Fi密码"
}

network={
ssid="单位Wi-Fi热点的名字"
psk="Wi-Fi密码"
}
```




把烧录好的RadioPi SD卡插回电脑，电脑会识别出来一个叫boot的驱动器。把写好的wpa_supplicant.conf文件，拷贝到它的根目录，弹出此盘，插给树莓派用即可。

在树莓派启动后，这个文件会自动更新RadioPi的Wi-Fi设置。然后它自行消失。

建议你把手机共享热点的信息提前写进去，以便于未来的野外操作。


## 结束语

至此，你已经具备了远程控制RadioPi的一切了。