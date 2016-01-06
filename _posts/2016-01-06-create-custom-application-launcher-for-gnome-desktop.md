---
layout: post
title: "Linux创建自定义的应用程序启动器"
description: ""
category:
tags: [linux, Gnome]
---
{% include JB/setup %}

eclipse在linux下，下载以后不需要安装，直接在目录下执行.\eclipse就可以运行了，不过同时，每次都要切换到该目录去执行eclipse似乎也比较麻烦。
又没有办法在应用程序菜单里找到，即使是gnome下，按super然后全局搜索，也找到这个。

不过好在linux是自由的系统，自己建一个应用程序的启动器一点问题都没有。

比如我的eclipse装在```/opt/eclipse-jee/```下,怎么新建一个启动器呢？

所有用户共享的应用启动器的目录是```/usr/share/applications/```目录，那么用户自定义的启动器目录在哪里？找了下用户目录下的.config/，没有找到，只好去百度google了一下，原来启动器是在.local/下，具体是```.local/share/applications```.

新建一个eclipse.desktop的文件，然后输入以下内容：

    [Desktop Entry]
    Encoding=UTF-8
    Name=Eclipse J2ee
    Comment=Official desktop version of Telegram messaging app
    Exec=/opt/eclipse-jee/eclipse
    Icon=/opt/eclipse-jee/icon.xpm
    Type=Application
    Categories=Development;

Categories就是类目，可以输入多个。编辑保存，不需要再做任何操作，eclipse就可以搜索到了。


参考：
[How do you create a custom application launcher in Gnome Shell? ](http://askubuntu.com/questions/112186/how-do-you-create-a-custom-application-launcher-in-gnome-shell)
