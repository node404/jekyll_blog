---
layout: post
title: "Linux普通用户sudo命令去掉密码提示"
description: ""
category:
tags: [linux, ]
---
{% include JB/setup %}



linux非root用户，默认情况下每次输入sudo都需要输入密码，这是出于安全考虑的设计无可厚非，不过有时候我们搞linux纯粹是为了娱乐，特别是有时候搞了个虚拟机还要每次用sudo都得输入密码神烦，于是就改下配置吧。
这其实很容易。linux的配置文件都大同小异，不过因为passwd, sudoers这种系统内置的特别是权限的文件比较重要，所以配置起来要比较小心一些，记得备份。

要让linux用户使用sudo不需要密码，可以对组指定，也可以对单个用户指定：

对用户组来说，只要把/etc/sudoer里追加%group ALL=(ALL) NOPASSWD:ALL即可
对于单个用户，可以直接在把/etc/sudoer里追加一行 username ALL=(ALL) NOPASSWD:ALL.

嗯，只是很简单的东西。算是给linux相关的东西开一个小头吧。


![nosudo](http://anton.logvinenko.name/content/sudo_no_password.png "nosudo")
