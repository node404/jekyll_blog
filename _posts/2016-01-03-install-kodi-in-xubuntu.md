---
layout: post
title: "在Xubuntu上安装并开机进入Kodi媒体中心"
description: ""
category:
tags: [ubuntu, kodi, XBMC]
---
{% include JB/setup %}

把2G的硬盘拆到了一台旧电脑上，并且给旧电脑装了xubuntu,打算直接打造一个媒体中心。目前知道有里那个用的不错的东西，KODI（XBMC）是一个很不错的选择。当然用PLEX也是个不错的选择，整理资料库方面PLEX更加强大。不过KODI是开源和免费的，既然用了linux，就用个开源的吧。况且我发现kodi还有不少的好处，并且更加轻量级。

其实Kodi有个基于ubuntu的发行版叫做kodibuntu，直接装得话效果其实也差不多。但是我还是喜欢从ubuntu去扩展，毕竟任何基于ubuntu的版本，相对ubuntu的开发肯定是有所滞后的。和Android的Rom一个道理。不过kubuntu因为是非常流行的发行版不用担心这个问题。

下面的步骤基本上是参考了askubuntu的一个答案[Autostart Kodi on Vivid](http://askubuntu.com/questions/596839/autostart-kodi-on-vivid)，只不过它是在纯净的server版本安装，而desktop版则因为有自身的启动器，需要将其禁用掉而已。

1）安装kodi: sudo apt-get install kodi

2）创建一个kodi的用户，并添加到和视频播放有关的组
    sudo adduser --disabled-password --disabled-login --gecos "" kodi
    sudo usermod -a -G audio kodi
    sudo usermod -a -G video kodi
    sudo usermod -a -G input kodi
    sudo usermod -a -G dialout kodi
    sudo usermod -a -G plugdev kodi
    sudo usermod -a -G tty kodi

3) sudo dpkg-reconfigure x11-common 弹出的选项框选择任何人（anybody）

4) sudo vim /etc/systemd/system/kodi.service
输入以下内容：

    [Unit]
    Description=Job that runs Kodi
    After=default.target graphical.target getty.target sound.target

    [Service]
    User=kodi
    Restart=always
    RestartSec=1s
    ExecStart=/usr/bin/xinit /usr/bin/kodi --standalone -- -nocursor

    [Install]
    WantedBy=default.target

5) 开机启动kodi的服务， 并禁止启动lightdm的服务

    sudo systemctl daemon-reload
    sudo systemctl disable lightdm
    sudo systemctl enable kodi

6) 重启

    reboot

下次电脑开机就可以直接进入kodi的界面了
