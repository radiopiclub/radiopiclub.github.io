---
layout: document
title: RadioPi使用说明
permalink: "/zh/documents-20200725.html"
author: BG6LH
version: 20200725
language: zh

---

# RadioPi使用说明

{% if page.version and page.author %}
- <small>Version: {{ page.version }}</small>
- <small>by {{ page.author }}</small>
{% endif %}

## 1. RadioPi简介

![radiopi-desktop](/img/radiopi-desktop.png)

如果你想玩树莓派远程控制电台，但是又苦于Linux系统太繁琐，那你可以试试RadioPi。这是一个配置好了的树莓派操作系统懒人包，安装了常用的HAM软件，并且针对<b>“远程控制”</b>和<b>“野外操作”</b>做了优化。你只要下载了它，烧个SD卡，往树莓派里一插，开机就能用。

*（为简化表达，接下来我们把运行RadioPi的树莓派电脑简称为RadioPi。）*


### RadioPi能干这些事

- 控制电台，玩FT8，记通联日志，上传LOTW挣DXCC积分。
- 一边在公园遛弯儿，一边用手机遥控家里电台。
- 去野外通联不用带笔记本电脑了。

我们三个搞互联网的HAM，BG6LH、BG1TPT、BI1EIH，结合自己使用树莓派的体验，一起做了这个RadioPi系统镜像。 如果你也想体验这些乐趣，下载一个试试吧。


## 2. RadioPi下载

{% include downloads_20200725.html %}

如果你了解SHA校验，可以比对一下校验码，确保下载的文件准确无误。

注意：刚下载的RaidoPi镜像是一个zip压缩包，烧卡之前要先解压缩，得到一个后缀是.img的文件。这个就是RadioPi镜像文件。


## 3. RadioPi的特色

RadioPi镜像是基于树莓派官方操作系统的再发布版本。我们是有洁癖的工程师，努力把它做的跟官方镜像一样干净，除了HAM常用的软件，别的都没装。

### 3.1 RadioPi特色功能

{% for features in site.data.features_20200725 %}
	{%- for feature in features.feature -%}
- {{ feature.zh }}
	{%- endfor %}
{% endfor %}


### 3.2 预装的软件

| 软件 | 版本 | 简介 |
| ---- | ---- | ---- |
{{ "" }}
{%- for software in site.data.softwares_20200725 -%}
| {{ software.name }} | {{ software.version }} |
    {%- for description in software.descriptions -%}
      {{ description.zh }}
    {%- endfor -%}
|
{% endfor %}


这些软件需要你自己完成开机设置，比如与电台的连接、呼号和LOTW账号等等。我们希望RadioPi能满足Ham的核心需求，也希望它尽量稳定，所以一些软件是最新的，一些是次新的。欢迎各位推荐更好用的软件，我们在以后的版本里预装。


### 3.3 RadioPi的默认用户名和密码

- 用户名：pi
- 密码：radiopi599


### 3.4 RadioPi的默认主机名

- 主机名：radiopi

RadioPi配置了自动广播主机名的Avahi服务，你可以在支持mDNS协议的其它设备上直接访问radiopi.local找到它。详情请参考《无头操作RadioPi》。

我们把RadioPi设置为开机自动登录到桌面，这是为VNC远程控制做的一个优化。第一次登录后，建议你立刻修改用户pi的密码。


## 4. 使用前的准备

使用RadioPi之前，需要做的一些准备：
### 4.1 microSD卡

- microSD卡，也叫TF卡、手机存储卡，推荐8G以上高速卡。
- USB的TF卡读卡器，或者TF转标准SD卡的卡套，烧卡用。

### 4.2 制卡软件

- Windows下，推荐使用[Win32DiskImager](https://sourceforge.net/projects/win32diskimager/)。
- MacOS下，推荐使用[balenaEtcher](https://www.balena.io/etcher/)。

### 4.3 树莓派
- 推荐购买Pi 4 model B，至少2G内存的版本。
- 如果你的树莓派不带Wi-Fi，建议买一个USB Wi-Fi网卡。
- 还可以准备一个USB GPS，获得更高精度的时间。这需要在radiopi上做一点点额外的配置。

### 4.4 其它准备

- 此外，根据电台的情况，你可能还需要准备一个USB声卡、以及USB转RS232或者TTL的控制设备。比如YAESU FT-817就需要这些，而ICOM IC-7300就不用，直接用USB线接RadioPi和电台就可以了。
- 显示器、键盘、鼠标，给树莓派用。

你也可以不用显示器、键盘和鼠标，以“无头(headless)”的方式使用RadioPi。RadioPi已经配置好了SSH、VNC、Avahi等等服务，并针对远程控制做了优化。所谓**“远程控制电台”**就是通过这些实现的。这才是玩树莓派的重点。关于这部分请参考下7节：[“远程控制RadioPi”](#7-远程控制radiopi)。



## 5. 给RadioPi联网

### 5.1 有线局域网

这是最简单的：**用网线把RadioPi和家里的路由器连起来。**



### 5.2 Wi-Fi无线局域网

在RadioPi的屏幕上，点击右上角的Wi-Fi图标，可以添加无线网络。这和普通电脑一样简单。



### 5.3 预存的热点连接

为了方便在野外“无头操作”，我们在RadioPi里预先保存了一个叫“radiopi”的Wi-Fi网络连接。只要你在手机上共享一个叫“radiopi”的热点，RadioPi开机就会自动连接上来。这个Wi-Fi连接的详细信息：

- ssid: radiopi
- 密码: radiopi599
- 加密方式：WPA-PSK

在iPhone、iPad上，共享热点的名称就是它的主机名。需要在“设置/通用/关于本机”中，把名称改成radiopi。

### 5.4 手动给RadioPi添加Wi-Fi

更技术性的方式，是直接在RadioPi的SD卡上放入Wi-Fi配置文件：

- 文件名：wpa_supplicant.conf
- 格式：txt文本

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


## 6. 在局域网里找到RadioPi

当你想实现从厨房或者浴室，控制阳台上的RadioPi和电台时，你要先知道RadioPi的局域网IP地址。以下介绍三种方法在局域网找到RadioPi。

### 6.1 在路由器上找radiopi

最简单方法是到路由器上看看分给它的IP地址是什么。RadioPi的默认主机名是小写的radiopi。找到它对应的IP。

![在路由器上找到radiopi对应的IP地址](/img/router_ip.png)

### 6.2 扫描IP地址

如果你无法登录路由器，还可以用IP扫描工具在局域网里扫出radiopi的IP地址。这里推荐的软件是：[**IP Angry Scanner**](https://angryip.org/download/)，它把你的电脑所在的局域网整个扫一遍，列出来结果。

![在Angry IP Scanner中，扫到radiopi主机的IP地址](/img/angryip-scan.png)

它扫到了RadioPi的IP地址是192.168.32.209。


### 6.3 直接访问radiopi.local

在支持mDNS的设备上，比如苹果的电脑、手机、iPad，可以直接用网址`radiopi.local`访问到RadioPi。



我们在RadioPi上配置了支持mDNS(多播域名)协议的Avahi服务。简言之，当RadioPi联网后，Avahi就在局域网内广播它的主机名，方便你找到它。RadioPi的默认主机名是小写的`radiopi`，所以它的局域网网址就是：`radiopi.local`。

下图以VNC Viewer为例，在地址栏中直接输入radiopi.local即可访问。

![在VNC Viewer地址栏直接输入radiopi.local](/img/vnc-link-local.png)

| **在不同类型操作系统上的mDNS服务**                           |
| ------------------------------------------------------------ |
| <sup>遗憾的是这种mDNS服务并不是所有操作系统的标准服务。<br /><br />**在苹果电脑或手机上使用Bonjour服务**<br />在苹果公司的设备上有Bonjour服务，它和Avahi一样支持mDNS协议。所以苹果的设备都可以直接访问radiopi.local。 <br /><br/>**在Windows上使用Bonjour服务**<br />微软的Windows系统中，默认是没有Avahi或者Bonjour服务的。有一个简单的方法：安装苹果公司的Bonjour打印服务程序，即可使Windows拥有类似的功能。Bonjour打印服务Windows版的下载地址：[https://support.apple.com/kb/DL999](https://support.apple.com/kb/DL999) <br /><br />**在linux系统上使用avahi服务**<br />在各种Linux类的电脑上，和树莓派一样要安装支持mDNS的服务avahi-daemon，过程这里不再赘述。<br /><br/>**在安卓手机上……**<br />安卓手机不在用户的操作层面提供mDNS服务。所以需要在路由器上查找，或者扫描局域网IP的方式，获取到radiopi的IP地址。 此外，RealVNC提供一种“云连接”的方式访问RadioPi。在完成设置之后，可以简化查找IP地址的过程。稍后会介绍。 </sup> |

## 7. 远程控制RadioPi

### 7.1 如何实现远程控制？

把RadioPi和电台连接在一起，再通过VNC这类远程桌面软件，从另一处的电脑或者手机上对RadioPi进行操作，从而实现对电台的控制。远程控制有两种情形：

- **局域网远程控制：**比如在家里，你拿着手机，在厨房或者浴室，对阳台上的RadioPi和电台实施“远程控制”。
- **互联网远程控制：**从公园控制家里的RadioPi和电台，或者一个城市控制另外一个城市的RadioPi和电台，这需要借助互联网实现。



### 7.2 关键工具：VNC

VNC是Virtual Network Console的缩写，虚拟网络控制台。简言之，它类似QQ的“远程协助”，能把RadioPi“投射”到另外一台电脑上或手机上，实现远程操作。就像下边这张图表达的一样。

![手机远程控制树莓派的示意图](/img/raspberry-pi-virtual.png)

我们在RadioPi上配置好了VNC Server以及相关的服务。接下来还需要你在另外一台电脑或者手机上，安装VNC Viewer。

#### 7.2.1 VNC Viewer下载地址：

- [https://www.realvnc.com/en/connect/download/viewer/](https://www.realvnc.com/en/connect/download/viewer/)
- 对于安卓手机用户，国内的应用商店可能没有。推荐从RealVNC公司直接下载[.apk安装包](https://help.realvnc.com/hc/en-us/articles/360002762697)。



### 7.3 通过局域网控制RadioPi

在VNC Viewer的地址栏，输入RadioPi的网址。

![在VNC的地址栏输入RadioPi的IP地址](/img/vnc-ip-address-2672600.png)



第一次与RadioPi建立连接时，VNC Viewer会有一个安全提示，选择Continue继续。

![第一次连接VNC的安全提示](/img/vnc-checkid.png)接下来VNC Viewer会提示你输入登录radiopi.local的用户名和密码。我们用RadioPi的默认用户pi登录。

- 用户名：pi
- 密码：radiopi599

![输入登录RadioPi的用户名密码](/img/vnc-login.png)

然后你就会在VNC Viewer的窗口里看到RadioPi的桌面。你可以像操作普通的电脑一样操作它了。至此，你已经实现了局域网内的RadioPi远程控制。

![第一次登录到RadioPi的桌面](/img/vnc-logined.png)



### 7.4 通过互联网控制RadioPi

如果想实现更远的(跨互联网)远程控制——比如从公园里控制家里的RadioPi——那就需要注册RealVNC提供**“云连接(Cloud Connect)”**服务。

![通过互联网访问家里的VNC Server 取自realvnc.com](/img/get-connected-internet-1.png)


你需要在去公园之前，要完成以下操作：

- 访问网站 [https://manage.realvnc.com](https://manage.realvnc.com) ，注册一个RealVNC账号。
- 在RadioPi的VNC Server上登录这个账号。
- 在手机的VNC Viewer上也登录这个账号。



每台设备**第一次登录**时，RealVNC都会要求用电子邮件进行安全验证。验证完之后，远程的RadioPi就会出自动现在VNC Viewer的地址簿中，下次访问时直接点击它就好。

![建立云连接之后的VNC Viewer窗口](/img/vnc-teambook.png)



## 8. 高级攻略：野外操作RadioPi

终于说到了野外通联。无头的RadioPi和手机或者Pad配合起来，可以完全取代笔记本电脑。

我们仍然要依赖VNC提供的服务，这意味着还要让RadioPi联网。野外最佳的联网手段，就是用手机共享一个Wi-Fi热点。所以我们在RadioPi里预先保存了一个叫“radiopi”的无线网络连接：

- ssid: radiopi
- 密码: radiopi599
- 加密方式：WPA-PSK

只要你在手机上共享一个叫“radiopi”的热点，把密码设置为：radiopi599，RadioPi开机就会自动连接上来。

在iPhone、iPad上，共享热点的名称就是它的主机名。需要在“设置/通用/关于本机”中，把名称改成radiopi。



RadioPi联网之后，iPhone可以直接用VNC Viewer访问radiopi.local，安卓手机用户可以访问“云连接”地址簿里的RadioPi。



![一套RadioPi和FT-818的便携集成](/img/ft818_gopack.jpg)



## 9. 关于RadioPi的音频

### 9.1 音频输入

正如您所了解的，目前的树莓派电脑都没有音频输入的硬件。所以对于那些没有声卡的电台，比如FT-817，需要另外准备一个USB声卡。电台的收到的声音信号，通过声卡的Line-in或Mic接口输入给RadioPi。RadioPi上的解码软件，比如WSJT-X、Fldigi等，再把这个声音解码。WSJT-X等软件发射信号时，调制后的音频也会从USB声卡直接传递给电台去发射。

### 9.2 音频输出

在默认情况下，树莓派的多个音频设备之间是相互独立的。如果你使用了外置的USB声卡，那么你可能在树莓派的3.5mm音频插孔上，无法同时监听到WSJT-X的声音。尽管这个需求不是那么迫切，我们还是准备了一个解决方案。我们在RadioPi上预装了PulseAudio Preferences。它可以将音频的输出同步到所有的声卡。

- 在RadioPi的开始菜单/首选项中打开PulseAudio Preferences(PulseAudio属性)。
- 选择 Simultaneous Output(同步输出)。
- 勾选“在所有本地声卡中为模拟输出添加虚拟(V)输出设备”。
- 重启WSJT-X等软件，将音频输出设备选为“combined”即可。


## 10. 安全提示

- RadioPi上的软件大部分基于Hamlib开源工具库，已经发展了二十年，成功且稳定。但是这些软件目前仍在更新，所以没人保证它们和每一个软件、每一个电台都能有完美的配合。
- 树莓派的硬件并非专门为射频工况设计，所以使用树莓派控制电台有一定的风险。当你用它和大功率电台、非平衡天馈系统一起工作时，不良的驻波、不佳的平衡与接地，都有可能对树莓派、电台及周边设备带来射频干扰。
- 我们使用过的电台有限，主要包括过FT-817/818、IC-7000/7300、D4D等，使用的天线包括端馈的长线、PAC12、小G5RV、甚至小环天线。无论是QRO还是QRP操作，都有过上述的经历。
- 所以认真对待平衡、接地、驻波等问题是非常必要的，尤其在远程操作无人值守的电台时更要注意这些问题。
- 解决这些问题也是玩业余无线电的乐趣之一。

## 11. Popularity Contest

我们想了解在RadioPi上哪些软件是最常被用到的，所以我们在RadioPi上默认安装了Debian的“人气竞赛(Popularity Contest)软件包。该软件包每周会向Debian Popcon项目的中央服务器提交一次RadioPi用户安装的软件包信息，且不包含用户的隐私信息。你也可以访问网站[https://popcon.debian.org](https://popcon.debian.org)了解统计结果。你完全可以自己决定是否退出“人气竞赛”，届时使用以下命令删除这个软件包即可：`sudo apt remove popularity-contest`。

## 12. 使用协议

- 下载和使用RadioPi镜像表示你已同意自行承担使用它的风险和责任。
- 使用RadioPi须遵守创作共享[CC BY-SA (署名-相同方式共享 4.0)](https://creativecommons.org/licenses/by-sa/4.0/deed.zh)协议。


