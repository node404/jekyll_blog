---
layout: post
title: "把用户态systemd服务改成系统服务，开机自启动xware"
description: ""
category:
tags: [Linux, torrent]
---
{% include JB/setup %}

OK.. 怎么给xubuntu装个迅雷远程呢，迅雷的Linux版本Xware-Desktop，目前是很稳定的，而且有很详细的官方教程：
https://github.com/Xinkai/XwareDesktop/wiki/%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E

这个就是针对ubuntu的，所以原封不动的执行就行了。

但是我的需求有点不一样，迅雷xware版自己提供了几个开机自启动选项：
1）由用户态systemd托管
2）由用户态upstart托管
3）简单的自动启动。

这三个选项的特点就是都是以用户态启动，事实上，xware的配置文件也确实是保存在用户的HOME目录下的.xware-desktop里的。

但是我希望Linux开机的时候并不会自动登录，更何况我的linux是运行kodi的，我也不会去手动登录，因此用户态的服务是不会自动启动的。

但是我希望一开机，xware就自动打开，开始执行下载任务。因此依赖原来的代码显然不太符合我的需求，就自己写一个系统的service吧。

还好几乎不需要改变xware用户态的服务，就可以搞定。用户态的服务创建在用户目录的```$HOME/.config/systemd/user```下，代码是这样的：

    [Unit]
    Description=Xware Service
    After=network.target

    [Service]
    Type=simple
    ExecStart=/opt/xware-desktop/xwared --log-novomit

    [Install]
    WantedBy=default.target

而系统服务在Ubuntu 14.04上位于```/etc/systemd/system/```目录下, 因此可以先把这个拷贝过来，然后稍作修改即可，修改的文件如下所示：

    [Unit]
    Description=Xwared Service
    After=network.target

    [Service]
    Type=simple
    User=kodi
    Group=kodi
    ExecStart=/opt/xware-desktop/xwared --log-novomit

    [Install]
    WantedBy=multi-user.target

其实就是简单的加入了User和Group的选项而已，让系统知道以哪个用户启动，这样就可以了。
