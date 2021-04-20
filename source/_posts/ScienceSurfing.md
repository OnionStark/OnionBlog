---
title: 科学上网客户端配置教程
date: 2020-06-08 23:57:40
updated: 2020-06-08 23:57:40
categories: 服务器公告
password: 193e5ef49365ea99b12b3dba6914204ffb761d445db179bb8ff654bd72d50000
top: true
---

# 服务器说明
　　服务器为AWS的Lightsail虚拟机，位于日本。科学上网的服务端现采用Trojan来实现，本文用于客户端的配置教程，同时如果翻墙方式有变动，都会在本文进行公布，接下来步入正题。

#  申请账号
　　为了方便管理，现每个人使用Trojan来科学上网前请申请自己的专属账号，用于密码验证。
　　申请网址：[onionstark.top/config](https://onionstark.top/config), 点击进入网站，界面如下所示：
{% asset_img trojanconfig.png config %}
　　点击右上角**REGISTER**注册账号，注册界面如下：
{% asset_img register.png 注册界面 %}
　　使用电子邮箱注册，并务必牢记用户名和密码。
　　点击首页右上角的Login登录账号，使用之前注册的电子邮箱和密码登录，登录成功后，界面如下所示：
{% asset_img login.png 登录界面 %}
　　**注意：**新注册的用户，Quota项为0，此时账号属于不可用状态，所以**务必注册成功后联系我开放权限**。

#  客户端配置

　　以下针对Windows、ios、Android客户端分别介绍如何配置Trojan进行科学上网。

##  Windows客户端配置

### Clash for Windows

[Clash](https://github.com/Dreamacro/clash) 是一个使用 Go 语言编写、基于规则的跨平台代理软件核心程序。Clash目前有Windows、MacOS、Android等多个平台的GUI程序，支持SS/V2ray/Trojan多种协议，功能强大。

Clash for Windows是基于Clash内核的Windows平台客户端。对于喜欢Clash的用户来说，Clash for Windows堪称完美：SS/V2ray/Trojan通通支持，功能强大；界面美观，使用Surge类似的托管配置，导入选择节点就可以用。然而对于习惯了SS/SSR/V2ray等手动配置的用户来说，Clash有点让人不爽：YAML语法稍不注意缩进就错了，需要半GUI半手动编辑配置文件，有点蛋疼。

本文介绍Clash for Windows配置trojan教程，让trojan用户彻底摆脱官方简陋的客户端。

首先，在[Clash的github下载页](https://github.com/Fndroid/clash_for_windows_pkg/releases)下载最新版程序，

![image-20200831135627723](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020083113-56-33-f0efc58a9f78c3d6c2e5ef6f6be58033-image-20200831135627723-2030a3.png)

如github访问不了可以此处[网盘下载](https://pan.baidu.com/s/1JXmpRW-4kOcxsNkkUTJuuw)提取码：iivy。

下载完成后双击进行安装，安装后打开，桌面右下角的托盘内找到Clash的图标(图标是一只猫)，点击，打开软件主界面：

![image-20200831135820713](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020083113-58-20-9d9a104c93879500554621e515b4944f-image-20200831135820713-978967.png)

然后点击[这里](https://pan.baidu.com/s/1Awavklhlt6OeceXWY8lC-A)下载配置文件，提取码为y810。用记事本、VS Code、Notepad++等编辑器打开，找到trojan配置块，如下图所示：

![image-20200831140348361](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020083114-03-48-aa24ac1fd8e6d9489d642a1fc13a5fc3-image-20200831140348361-f1b7e2.png)

服务器地址和端口已经填好，只需要填password一项。password为username:passwd	

　　username为你第一步注册时设置的**用户名**，passwd为你设置的**密码**，**记得中间用 : 隔开！！！**（切记是**英文的冒号**不是中文的冒号）

填好后保存，打开clash客户端点击“Profiles”，把修改好的配置文件拖到clash界面中，然后双击选中拖进来的配置文件(深色表示选中)：

![image-20200831140724057](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020083114-07-24-1ab577cd664b31c3bd9ff496fd8450a3-image-20200831140724057-966bb8.png)

接着点击“Proxies”，进入最重要的设置：选择代理模式和选择使用的节点：

![image-20200920110122617](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020092011-01-28-f880bb3e984c055414aede82e2659d4b-image-20200920110122617-ddff97.png)

最上面的Global、Rule和Direct表示全局代理、基于规则路由和直连，(可以认为)对应其他客户端的全局模式、PAC模式和禁用系统代理。**绝大多数情况建议使用Rule**。

接着在PROXY里选择使用的节点，默认是自动选择快速节点，在这里可以指定刚添加的trojan节点。

Final表示如果没有匹配的规则，默认走代理还是直连，个人建议默认直连，网站打不开再选择代理。

其他看不懂的东西保持默认就好了。

最后点击General回到主界面，点击“System Proxy”，开启系统代理：

![image-20200831140958255](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020083114-09-58-7dab9433ca58c7c120130eec0240c91d-image-20200831140958255-5595db.png)

Start with Windows开启后可以随系统启动。

配置无误的话，接下来就可以顺利上外网了。

出于节约流量的角度，平时建议用Rule模式：

![image-20200831141137385](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020083114-11-37-c8dbf51371963ee07be5991846694803-image-20200831141137385-f05bf8.png)

当浏览的小网站打不开时再切换Global或者自己向Rule里添加该网址。

##  ios客户端配置

　　ios客户端使用Shadowrocket，该app只能在美区付费下载。可以联系我提供账号免费下载。

　　打开程序后点击右上角的+号新建连接，界面如下：
<div style="width: 200px; margin: auto">{% asset_img shadowrocket.PNG shadowrocket新建连接 %}</div>
　　需要改动的地方有：

　　**类型**：Trojan

　　**服务器**：onionstark.top

　　**端口**：443

　　**密码**：username:passwd	

　　username为你第一步注册时设置的**用户名**，passwd为你设置的**密码**，**记得中间用 : 隔开！！！**（切记是**英文的冒号**不是中文的冒号）

##  安卓客户端配置

　　安卓客户端为[igniter](https://github.com/trojan-gfw/igniter/releases)，同样为了避免github下载速度过慢，提供了网盘下载连接：[igniter](https://pan.baidu.com/s/1qRApF3V69MoQt5TrsGSOkg)，提取码：bngu
　　打开app，界面如下：
　　<div style="width: 200px; margin: auto">{% asset_img igniter.jpg igniter界面 %}</div>
　　需要改动的地方有：

　　**第一行（服务器地址）**：onionstark.top

　　**第二行（端口）**：443

　　**第三行（密码）**：username:passwd	

　　username为你第一步注册时设置的**用户名**，passwd为你设置的**密码**，**记得中间用 : 隔开！！！**（切记是**英文的冒号**不是中文的冒号）

