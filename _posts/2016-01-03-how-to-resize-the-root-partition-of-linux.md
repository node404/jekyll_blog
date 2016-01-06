---
layout: post
title: "调整linux主分区的大小"
description: ""
category:
tags: [linux, partition]
---
{% include JB/setup %}

在安装我笔记本商的archlinux的时候，root分区和主分区是分开的，后来为了尝试KaliLinux，就把home分区牺牲掉，放弃了/home的单独分区。可是如今60G的空间已经不够用了。

于是打算重新调整一个root分区的大小。我分区之前，256G硬盘的分区表大概是这样的，boot分区在最前，大概500M，，home分区紧随其后，66G左右，然后是8G的swap交换空间。还剩下170G左右的剩余空间。

我的目标是把swap分区弄到最后，其他都扩展给HOME分区。

依稀记得装arch的时候，swap并不是多大不了的事情，甚至说可以不需要swap。于是我打算先把swap禁用掉，然后删除。再重新创建swap分区，然后resize以下Root分区的大小基本就可以大功告成了。不过调整root分区的大小是不可以在当前系统下完成的。因此很有必要弄一个usb的镜像，通过镜像系统进去后调整没有mount的swap分区。

在linux下把镜像刻录到usb是非常方便的。接上U盘以后，可以通过```lsblk``` 查看一下u盘的设备路径,比如我的是```/dev/sdc```,基本上可以通过大小就能判断出来了。

    sdc                   8:32   1   7.2G  0 disk
    ├─sdc1                8:33   1   1.1G  0 part
    └─sdc2                8:34   1   2.2M  0 part

然后找到镜像的位置，输入以下命令，耐心等待几分钟就可以了。我因为之前刚刚用过kubuntu，所以也懒得在刻录别的。其实要实现分区大小调整，基本上只要能跑起来的发行版都可以。不过ubuntu下有个预装的工具叫```gparted```，可以免掉一些麻烦。

sudo dd bs=4M if=/data/OS_ISOs/Linux/xubuntu-15.10-beta2-desktop-amd64.iso of=/dev/sdc

几分钟之后就刻录完成。

接下来在arch上，先把swap分区禁用掉，还是通过```lsblk```, 记录/root分区和swap分区是哪个。

    ─sda2                8:2    0  66.4G  0 part /
    ─sda4                8:4    0     8G  0 part [SWAP]

我的是```/dev/sda4```, 禁用swap:

    sudo swapoff /dev/sda4

然后编辑分区表。

    sudo cp /etc/fstab /etc/fstab.bak # 备份下，万一弄错了还可以恢复系统
    sudo vim /etc/fstab

在swap的那个设备前面加一个"#"，注释掉swap的分区。

接上u盘，重启。因为是Xubuntu，所以选择```try xubuntu```以后，非常友好的进入的gui的界面。

然后只要在“开始”菜单（原谅我的windows叫法），找到```gparted```，一切就很容易了。

在/dev/sda4上右键，选择delete，然后new一个swap，位置选择在硬盘的最后(gparted因为直接有示意图，可以直接在上面选择)，新的swap分区大小自然也和老的swap一致，一般都是推荐和内存一样。接着在/dev/sda2上右键，选择resize，直接用光所有的硬盘空间。确定，耐心等待一分钟左右，就会弹出'Done'的提示。

然后重启，重新进入archlinux，这时候如果系统成功进入，就说明重新分区已经成功了。

但是swap分区还并没有启用。执行```bklid```, 确定swap分区的uuid，然后重新编辑分区表:

    sudo vim /etc/fstab

把之前swap分区的uuid改成新的uuid，保存。然后根据swap的位置执行后面两条命令，启用swap，新的设备名称可能还是```/dev/sda4```也可能不是。这时候可以输入```sudo bklid ```来确定以下：

    sudo mkswap /dev/sda4
    sudo swapon /dev/sda4

重启，reboot。重启以后
