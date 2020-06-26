---
layout: document
title: RadioPi使用说明
permalink: "/en/documents.html"
author: BG6LH
version: 20200621
language: en

---

# RadioPi User Guide

{% if page.version and page.author %}
- <small>Version: {{ page.version }}</small>
- <small>by {{ page.author }}</small>
{% endif %}


## 1. Introduction

![radiopi-desktop](/img/radiopi-desktop.png)


RadioPi is a Hamradio Raspberry Pi OS image. You can flash it into a SD card, and run it on a RPi computer directly.

*(For the convenience of expression, we will refer to the RPi computer running RadioPi image as **"RadioPi"** in short.)*


RadioPi can help you to:
- Control your rig, run FT8, log QSO, and upload to LoTW. 
- Access your rig@home by your mobile phone, when you take a walk in a park.
- Go to field operation without your laptop computer.



## 2. Features of RadioPi

The RadioPi image is a redistribution based on the Raspberry Pi OS. We try to make it as clean as the official image. 

### 2.1 Softwares on RadioPi
<ul>
{% for software in site.data.softwares %}
<li><b>{{ software.name }}</b> - {{ software.version }} - {{ software.dscription_en }}</li>
{% endfor %}
</ul>

You need to complete some the power-on settings by yourself, such as the connection with your rig, call sign and LoTW account and so on. We hope that RadioPi can meet Ham's requirement as stable as possible, so some software is up to date, some are stable. You are welcome to recommend better software, we will pre-install it in future radiopi versions.


### 2.2 Configuration

- Removed non-ham softwares
- Mariadb for CQRLog, and the SSL bug was fixed
- SSH, VNC, Avahi services were enabled
- A hotspot "radiopi" was installed for field operation
- Chorny for ntp service


### 2.3 Default username and password of RadioPi

- username: pi
- password: radiopi599

Those are username and passwords login to SSH, VNC, or desktop.

### 2.4 Default host name of RadioPi

- hostname: radiopi


RadioPi is configured with the Avahi service that automatically broadcasts hostnames in LAN networking. You can find your RadioPi by directly accessing radiopi.local on other devices that support the mDNS protocol.

RadioPi is configured to automatically login to the desktop after booting, which is an optimization for VNC remote control. After your first login, it is recommended that you change the password of user pi.


## 3. Download

**中文版下载地址：**[https://pan.baidu.com/s/17HxJHYbyRwxaK_dh4-24WA](https://pan.baidu.com/s/17HxJHYbyRwxaK_dh4-24WA)
<br />**提取码：**`02e7`
<br/>**SHA-256：**`99ab3ec6125cc07d878746d810a1d74db3db7bada44e5373ae1b7681da3e07f3`



## 4. Preparation before use

Before using RadioPi, you need to do some preparations:


### 4.1 microSD cards

- MicroSD cards, also called TF cards, mobile phone memory cards, recommend high-speed cards at least 8 GB.
- USB TF card reader, or TF to SD convertor, for writing cards.

### 4.2 Card writing softwares 

- On Windows, [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/) is recommended.
- On MacOS下, [balenaEtcher](https://www.balena.io/etcher/) is recommended.

### 4.3 Raspberry Pi

- Pi 4 model B is recommended, at least 2G memory version.
- If your Raspberry Pi does not have a Wi-Fi network, it is recommended to buy a USB Wi-Fi dongle.
- You can also prepare a USB GPS dongle to get more accurate time. This requires a little extra configuration on radiopi.


### 4.4 Other preparations

- In addition, depending on your rig, you may also need a USB sound card dongle, a USB to RS232 or TTL convertor, and so on. For example, YAESU FT-817 may need those perpherals. However, ICOM IC-7300 does not need them. It connect to RadioPi directly with its USB cable.

- The monitor, keyboard, and mouse.

You can also use the RadioPi in "headless", without using a monitor, keyboard and mouse. RadioPi has been configured with SSH, VNC, Avahi services, and is optimized for remote access. This is the point of playing the Raspberry Pi. For this part, please refer to the Part 7: ["Remote Access RadioPi"](#7-remote-access-radiopi).


## 5. Networking for RadioPi

### 5.1 Wired LAN

This is the simplest way: connect the RadioPi to your home router with a cable wire.


### 5.2 Wi-Fi Network

On the RadioPi screen, click the Wi-Fi icon in the upper-right corner to add a wireless network. This is as simple as a normal computer.


### 5.3 A Pre-saved Wi-Fi network

For field operation with a "headless" RadioPi , we have pre-saved a Wi-Fi network, named "radiopi", in it.
When you share a Wi-Fi hotspot named "radiopi" on your phone, the RadioPi will automatically connect it after power on. Details of this Wi-Fi network:

- ssid: radiopi
- psk: radiopi599
- key_mgmt: WPA-PSK

On iPhone and iPad, the name of the shared hotspot is its hostname. You need to change it to `radiopi` in "Settings/General/About/Name".


### 5.4 Setting up wireless networking Manually

One more technical way is writting a Wi-Fi configuration file on the RadioPi's boot folder:

- File name: wpa_supplicant.conf
- Format: txt

In this file, you can write multiple Wi-Fi networks. You will need to create a text file named `wpa_supplicant.conf`, paste the following codes, change it by your information:


```shell
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
ssid="radiopi"
psk="radiopi599"
}

network={
ssid="<Name of your home wireless LAN>"
psk="<Wi-Fi password>"
}

network={
ssid="<Name of your office wireless LAN>"
psk="<Wi-Fi password>"
}

```

Then:
- Putting your RadioPi SD card back to computer, there will be a new drive named "boot", it is RadioPi's boot folder.
- Copying the `wpa_supplicant.conf` to drive boot's root directory.
- Ejecting you SD card, and putting it into the Raspberry Pi.

After the Raspberry Pi is started, this file will automatically update the RadioPi's Wi-Fi settings.

We recommend you to keep a Wi-Fi network of your mobile phone's shared hotspot, for your future field operations.



## 6.  Find RadioPi in the LAN

当你想实现从厨房或者浴室，控制阳台上的RadioPi和电台时，你要先知道RadioPi的局域网IP地址。以下介绍三种方法在局域网找到RadioPi。

### 6.1 Find RadioPi on the router

最简单方法是到路由器上看看分给它的IP地址是什么。RadioPi的默认主机名是小写的radiopi。找到它对应的IP。

![在路由器上找到radiopi对应的IP地址](/img/router_ip.png)

### 6.2 Scan IP address

如果你无法登录路由器，还可以用IP扫描工具在局域网里扫出radiopi的IP地址。这里推荐的软件是：[IP Angry Scanner](https://angryip.org/download/)，它把你的电脑所在的局域网整个扫一遍，列出来结果。

![在Angry IP Scanner中，扫到radiopi主机的IP地址](/img/angryip-scan.png)

它扫到了RadioPi的IP地址是192.168.32.209。


### 6.3  Access radiopi.local directly

在支持mDNS的设备上，比如苹果的电脑、手机、iPad，可以直接用网址`radiopi.local`访问到RadioPi。



我们在RadioPi上配置了支持mDNS(多播域名)协议的Avahi服务。简言之，当RadioPi联网后，Avahi就在局域网内广播它的主机名，方便你找到它。RadioPi的默认主机名是小写的`radiopi`，所以它的局域网网址就是：`radiopi.local`。

下图以VNC Viewer为例，在地址栏中直接输入radiopi.local即可访问。

![在VNC Viewer地址栏直接输入radiopi.local](/img/vnc-link-local.png)

| **在不同类型操作系统上的mDNS服务**                           |
| ------------------------------------------------------------ |
| <sup>遗憾的是这种mDNS服务并不是所有操作系统的标准服务。<br /><br />**在苹果电脑或手机上使用Bonjour服务**<br />在苹果公司的设备上有Bonjour服务，它和Avahi一样支持mDNS协议。所以苹果的设备都可以直接访问radiopi.local。 <br /><br/>**在Windows上使用Bonjour服务**<br />微软的Windows系统中，默认是没有Avahi或者Bonjour服务的。有一个简单的方法：安装苹果公司的Bonjour打印服务程序，即可使Windows拥有类似的功能。Bonjour打印服务Windows版的下载地址：[https://support.apple.com/kb/DL999](https://support.apple.com/kb/DL999) <br /><br />**在linux系统上使用avahi服务**<br />在各种Linux类的电脑上，和树莓派一样要安装支持mDNS的服务avahi-daemon，过程这里不再赘述。<br /><br/>**在安卓手机上……**<br />安卓手机不在用户的操作层面提供mDNS服务。所以需要在路由器上查找，或者扫描局域网IP的方式，获取到radiopi的IP地址。 此外，RealVNC提供一种“云连接”的方式访问RadioPi。在完成设置之后，可以简化查找IP地址的过程。稍后会介绍。 </sup> |

## 7. Remote Access RadioPi

### 7.1 How to remote access

把RadioPi和电台连接在一起，再通过VNC这类远程桌面软件，从另一处的电脑或者手机上对RadioPi进行操作，从而实现对电台的控制。远程控制有两种情形：

- **局域网远程控制：**比如在家里，你拿着手机，在厨房或者浴室，对阳台上的RadioPi和电台实施“远程控制”。
- **互联网远程控制：**从公园控制家里的RadioPi和电台，或者一个城市控制另外一个城市的RadioPi和电台，这需要借助互联网实现。



### 7.2 VNC (Virtual Network Computing)

VNC是Virtual Network Computing的缩写，虚拟网络控制台。简言之，它类似QQ的“远程协助”，能把RadioPi“投射”到另外一台电脑上或手机上，实现远程操作。就像下边这张图表达的一样。

![手机远程控制树莓派的示意图](/img/raspberry-pi-virtual.png)

我们在RadioPi上配置好了VNC Server以及相关的服务。接下来还需要你在另外一台电脑或者手机上，安装VNC Viewer。

#### 7.2.1 Download VNC Viewer:

- [https://www.realvnc.com/en/connect/download/viewer/](https://www.realvnc.com/en/connect/download/viewer/)
- 对于安卓手机用户，国内的应用商店可能没有。推荐从RealVNC公司直接下载[.apk安装包](https://help.realvnc.com/hc/en-us/articles/360002762697)。



### 7.3 Remote Access RadioPi via LAN

在VNC Viewer的地址栏，输入RadioPi的网址。

![在VNC的地址栏输入RadioPi的IP地址](/img/vnc-ip-address-2672600.png)



第一次与RadioPi建立连接时，VNC Viewer会有一个安全提示，选择Continue继续。

![第一次连接VNC的安全提示](/img/vnc-checkid.png)接下来VNC Viewer会提示你输入登录radiopi.local的用户名和密码。我们用RadioPi的默认用户pi登录。

- username：pi
- password：radiopi599

![输入登录RadioPi的用户名密码](/img/vnc-login.png)

然后你就会在VNC Viewer的窗口里看到RadioPi的桌面。你可以像操作普通的电脑一样操作它了。至此，你已经实现了局域网内的RadioPi远程控制。

![第一次登录到RadioPi的桌面](/img/vnc-logined.png)



### 7.4 Remote Access RadioPi via internet

如果想实现更远的(跨互联网)远程控制——比如从公园里控制家里的RadioPi——那就需要注册RealVNC提供**“云连接(Cloud Connect)”**服务。

![通过互联网访问家里的VNC Server 取自realvnc.com](/img/get-connected-internet-1.png)


你需要在去公园之前，要完成以下操作：

- 访问网站 [https://manage.realvnc.com](https://manage.realvnc.com) ，注册一个RealVNC账号。
- 在RadioPi的VNC Server上登录这个账号。
- 在手机的VNC Viewer上也登录这个账号。



每台设备**第一次登录**时，RealVNC都会要求用电子邮件进行安全验证。验证完之后，远程的RadioPi就会出自动现在VNC Viewer的地址簿中，下次访问时直接点击它就好。

![建立云连接之后的VNC Viewer窗口](/img/vnc-teambook.png)



## 8. Advanced guide: RadioPi for field operation

终于说到了野外通联。无头的RadioPi和手机或者Pad配合起来，可以完全取代笔记本电脑。

我们仍然要依赖VNC提供的服务，这意味着还要让RadioPi联网。野外最佳的联网手段，就是用手机共享一个Wi-Fi热点。所以我们在RadioPi里预先保存了一个叫“radiopi”的无线网络连接：

- ssid: radiopi
- psk: radiopi599
- key_mgmt：WPA-PSK

只要你在手机上共享一个叫“radiopi”的热点，把密码设置为：radiopi599，RadioPi开机就会自动连接上来。

在iPhone、iPad上，共享热点的名称就是它的主机名。需要在“设置/通用/关于本机”中，把名称改成radiopi。



RadioPi联网之后，iPhone可以直接用VNC Viewer访问radiopi.local，安卓手机用户可以访问“云连接”地址簿里的RadioPi。



![a go-pack of RadioPi and YEASU FT-818](/img/ft818_gopack.jpg)



## 9. Safety tips

- RadioPi上的软件大部分基于Hamlib开源工具库，已经发展了二十年，成功且稳定。但是这些软件目前仍在更新，所以没人保证它们和每一个软件、每一个电台都能有完美的配合。
- 树莓派的硬件并非专门为射频工况设计，所以使用树莓派控制电台有一定的风险。当你用它和大功率电台、非平衡天馈系统一起工作时，不良的驻波、不佳的平衡与接地，都有可能对树莓派、电台及周边设备带来射频干扰。
- 我们使用过的电台有限，主要包括过FT-817/818、IC-7000/7300、D4D等，使用的天线包括端馈的长线、PAC12、小G5RV、甚至小环天线。无论是QRO还是QRP操作，都有过上述的经历。
- 所以认真对待平衡、接地、驻波等问题是非常必要的，尤其在远程操作无人值守的电台时更要注意这些问题。
- 解决这些问题也是玩业余无线电的乐趣之一。



## 10. Agreement

- 下载和使用RadioPi镜像表示你已同意自行承担使用它的风险和责任。
- 请确保自己在法律法规许可范围内操作计算机、无线电设备和互联网。
- RadioPi 镜像遵循“[创作共享 署名-相同方式共享 CC BY-SA 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh)”。

