---
layout: post
title: "replace nvidia driver with nouveau driver arch linux"
description: ""
category:
tags: [driver, archlinux,]
---
{% include JB/setup %}

昨天还装完arch兴高采烈，今天就差点放弃整个系统，罪魁祸首就是当年被linus喷了的nvidia.

Archlinux装了使用nvidia驱动，一开始发现根本就没有加载，装了和没装没啥区别，但是跑的好好的阿跑的好好的，结果不小心手贱点了```nvidia-xconfig```的应用，莫名其妙的起不来了。（这个原因我想了半天才想出来，不然我实在是无力吐槽他妈跑了一天多好好的什么也没干突然就起步来的情况，一度怀疑是显卡硬件坏了）

于是抢救三个多小时，```/var/log/xorg.0.log```里的报错也莫名其妙，比如:

    not detect device nvidia, no screen found.

简直是日到狗。于是想着解决办法应该是彻底不要装nvidia的驱动， 用intel的集成显卡，照例来说应该也ok阿，于是直接删掉驱动：

pacman -R nvidia nvidia-utils

然后reboot, 没想到发生的事情更加奇葩了，连命令行就出不来了，直接黑屏了我草。

心里十万个草尼马，还好安装过arch的人毕竟还是知道有个arch-chroot, 放入安装介质（我的是u盘），重启， mount各种好盘:

    mount /dev/sda2 mnt
    arch-chroot /mnt /bin/bash

然后咋办呢，突然在arch的官方教程里出现了一个词： nouveau（还好我还有台电脑阿，这时候如果只有一台电脑要在手机上查资料的话。。。我tm会不会放弃linux直接装个windows都不知道，我就是对自己那么没信心阿）

nouveau是个什么呢，简单的解释就是个nvidia的开源驱动。。。虽然还不是对其了解，但是还是对开源两个字报有信心，而且也是死马当活马医了。。。于是:

    pacman -S xf86-video-nouveau

据说还有什么3d的实验特性支持, (还可以```pacman -S mesa mesa-libg1```,  额这好像和我目前十万火急的状态没啥关系）

然后咋办呢。。。nouveau已经装好了，nvidia也已经卸载了，这样就ok了么，好像还不行

    lsmod  | gerp nvidia
    dmesg | grep nvidia

两个命令显示，nouveau还没有work，系统还是在用nvidia，继续查arch wiki 的 nouveau页，删除mod的方法是

    modprobe -r nvidia

然后加入nouveau的mod:

    modprobe nouveau

先就这样吧，从安装盘里推出来

    umount -R /mnt

然后reboot，重启试试， 尝下startx还是不幸， 并且我们在dmesg | grep nouveau里看到了nouveau报错。。。额什么情况：

    pointer to tmds table invalid...

实在不知道什么鬼，但是坚信一定是fuck的nvidia的问题，注意到startx时系统日志说了加载配置文件的路径/etc/X11/xorg.conf, 打开看看。。。一看吓一跳， 某个device里的driver,赫然写着nvidia这几个大字（于是就很显然了，一开始我打开nvidia-xconfig的时候，nvidia就是帮我做了这件事情，把xorg.conf文件给改了。更何况我还在/etc/X11目录下看到了一个文件名为xorg.conf.nvidia-xconfig-original的文件，我猜也知道这是nvidia-xconfig在改之前做的备份。。。（额，nvidia你居然知道做备份却不知道你的操作可能会导致用户进不了图形界面么），虽然打开来一看, 这个文件其实是空的，不过这大概这是巧合罢了吧，因为我之前也没有装过什么驱动。。。

于是我把这个文件还原回去，虽然还是觉得没什么希望，鬼知道nvidia还干了什么。reboot。。。startxfce4, 然后。。。图形界面回来了，亲爱的xfce...额

虽然我觉得可能一开始把那个xorg.conf文件改回来可能就解决问题了。。。折腾了一大堆的意义，几乎到了绝境快要放弃了，不断的尝试，事情还是总会给你惊喜的。而另一个收获是发现nouveau这个驱动，目前还不好说真正的效果到底比nvidia自己的驱动好还是不好，不过我坚定的表示继续用nouveau的驱动而不会用nvidia的驱动了。

说到这里不得不说arch社区和文档的强大。如果是别的发行版缺乏文档的情况下有时候尝试会越尝试系统越没救。

o了今天其实还拖着病体阿，早点去休息了，睡前亚冠决赛，恒大加油！

----------------------------------

最后故事的结局是, nouveau的驱动也并不稳定，依旧出现间歇性的黑屏，只能说不管是 nvidia和nouveau的驱动，都不能很好的支持我的显卡，估计是因为2015年的太新了，顺便报以下我的显卡：NVIDIA® GeForce® 920M 4GB DDR3。。。于是还是奉劝各位装linux尽量还是买个集成显卡的算了，别买nvidia和amd的了，一是没太大用，二是驱动出问题的可能太多，难以调试。

所以打算也干掉nouveau的驱动

pacman -R xf86-video-nouveau

然后```rmmod nouveau``` 或者 ```modprobe -r nouveau```, 确认 ```lsmod | gerp nouveau```  和 ```dmesg | grep nouveau``` 结果是————干不掉，reboot也没用。

原因是其实linux内核里其实是有nouveau的驱动的。怎么搞定呢，wiki里说有个东西叫modprobe的黑名单, 在```/etc/modprobe.d/```里新建一个文件```blacklist_nouveau.conf```， 写入:

    blacklist nouveau

重启，这样不管是

    lsmod | gerp nouveau
    dmesg | grep nouveau

还是

    lsmod | gerp nvidia
    dmesg | grep nvidia

都什么也查不到了，所名nvidia显卡没有用，系统现在用的是我的集成显卡。好吧，我的4g显存的显卡就这样完全闲置了。
