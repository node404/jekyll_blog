---
layout: post
title: "给kodi的kubuntu安装vncserver"
description: ""
category:
tags: [vnc, remote_desktop]
---
{% include JB/setup %}

服务器端：

1）在终端安装tigervnc：

    $ sudo apt-get install tigervnc

2）启动tigervnc

    $ vncserver

提示你输入vnc的密码：

    enter your password (length 8)

然后终端会显示New 'X' desktop is kodi-kodibuntu:1

3）（可选）如果需要改变桌面，例如不想用xvnc，可以编辑```$HOME/.vnc/xstartup```的内容。具体操作暂略。

4）（可选）如果需要开机启动，一个是用supervisor，另外就是在/etc/systemd/system/下手动写一个服务。具体操作暂略。

在客户端:

1) 安装tigervnc:

    $ sudo pacman -S tigervnc   (archlinux)

或者

    $ sudo apt-get install tigervnc (ubuntu/debian)

2) 直接运行vncviewer或者在终端输入:

    $ vncviewer

端口默认是5900+n，根据New 'X' desktop is kodi-kodibuntu:1, 因为服务器上的提示是:1, 所以端口就是5901

输入你的ip地址是端口: 192.168.1.xxx:5901

会提示让你输入密码：

进去后就是vnc的桌面了, 在xubuntu下测试，不需要配置服务器上的```$HOME/.vnc/xstartup```就可以直接进入xfce4桌面

---

更正，采用默认的.vnc/xstartup配置虽然能用，但是会有一些样式问题，比如图标无法显示。所以还是改一下好了：

    $ cp ~/.vnc/xstartup ~/.vnc/xstartup.bak
    $ vim ~/.vnv/xstartup

删除所有的内容，重启vncserver

    $ vncserver -kill :1
    $ vncserver

进去以后基本商就是和你直接进入桌面一样的xfce了。

## 开机自动启动






[How To Install VNC Server On Ubuntu 14.04](https://www.howtoforge.com/how-to-install-vnc-server-on-ubuntu-14.04)
