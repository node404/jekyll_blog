---
layout: post
title: "安装deluge服务器，并远程下载和添加torrent"
description: ""
category:
tags: [torrent, ubuntu]
---
{% include JB/setup %}
上一篇装完kodi后，我的旧电脑就彻底变成了一个kodi/XBMC媒体中心，那么比如我要下载新的美剧什么的，好像就还是不那么方便。
p2p的torrent一直是我的最爱。deluge是一个跨平台的操作系统，支持linux, windows 和mac，还有远程控制等功能，什么是远程下载呢。我们都知道有迅雷远程下载，那么deluge其实也是一样，就是你在一台电脑操作，有一个下载链接，然后你可以直接在这台电脑上添加这个下载链接，唯一不同的是下载的任务被添加到了远程的机器上，文件也会下载到远程的机器上。
这样的需求正符合我媒体中心的需要，我找到新的片源，并不需要下载到我的计算机，而是可以让我的Kodi在播放电影的同时，又可以执行下载的任务。


# 服务器端：

安装:

    $ sudo apt-get install deluged deluge-web

新建一个deluge用户和用户组:

    $ sudo adduser --system  --gecos "Deluge Service" --disabled-password --group --home /var/lib/deluge deluge

把用户添加到deluge用户组```adduser <username> deluge```，便于用户操作torrent和下载到的文件，在我的kodi的xubuntu，当然就是kodi用户了：

    $ sudo adduser kodi deluge

需要临时登录deluge用户配置远程管理, 因此如果系统不允许没有密码的用户登录，则需要给deluge设置一个密码

    $ sudo passwd deluge：

然后再登录deluge用户：

    $ su deluge

    $ deluge-console "config -s allow_remote True"
    $ deluge-console "config allow_remote"

修改远程控制的密码，格式是```<username>:<password>:10```：

    $ cp ~/.config/deluge/auth ~/.conf/deluge/auth.bak
    $ vim ~/.config/deluge/auth

退出deluge：

    $ exit

执行deluge的守护进程：

    $ deluged

(可选)让deluge作为服务运行，暂略。

# 客户端

现在回到我的笔记本上：

安装deluge：

    $ sudo pacman -S deluge

OR

    $ sudo apt-get install deluge

deluge安装好了，在终端执行deluge, 或者直接打开应用程序，在菜单中选择：

    Preferences -> Interface : 把 Classic Mode 的勾勾掉

然后会自动重启deluge，重新启动后，系统会让你选择一个服务器，默认有一个127.0.0.1的本地deluge服务，显然这不是我们想要的：
点击Add，输入服务器的ip地址:

Hostname就是服务器的IP地址.
Port端口一般都是默认的58846.
Username用户名和Password密码就是之前在auth文件里的配置

Ok. 完成，现在可以随便找个网站下个torrent文件测试下，kat.cr是我逛的最多的一个了。

Is that cool!

当然deluge还有更加厉害的地方，比如它可以自动检测某个目录下有了新的torrent文件，检测到以后会自动执行下载任务。这个特性让我想到了一些更加有趣的事情，下次再一一道来吧。

Document based on:

* [UserGuide/ThinClient – Deluge](http://dev.deluge-torrent.org/wiki/UserGuide/ThinClient)

* [UserGuide/Service/systemd – Deluge](http://dev.deluge-torrent.org/wiki/UserGuide/Service/systemd)

* [Installing/Linux/Ubuntu – Deluge](http://dev.deluge-torrent.org/wiki/Installing/Linux/Ubuntu)
