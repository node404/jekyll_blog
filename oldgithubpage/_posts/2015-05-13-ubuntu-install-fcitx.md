---
layout: post
title: "ubuntu install fcitx"
description: ""
category: linux
tags: [input_method, ubuntu]
---
{% include JB/setup %}

# fictix 安装fcitx

fictix拼音有fcitx-pinyin、fcitx-sogoupinyin、fcitx-sunpinyin、fcitx-googlepinyin，三种可以选择.五笔有fcitx-table、fcitx-table-wubi、fcitx-table-wbpy（五笔拼音混合）可供选择.


------------------------------

卸载ibus参考以下步骤:```实际上可以不卸载```

在终端中依次运行

killall ibus-daemon
sudo apt-get purge ibus ibus-gtk ibus-gtk3 ibus-pinyin* ibus-sunpinyin ibus-table python-ibus
rm -rf ~/.config/ibus

-------------------------------

接下来安装fcitx
sudo add-apt-repository ppa:fcitx-team/nightly
sudo apt-get update
sudo apt-get install fcitx-googlepinyin    //注:我安装的是Google拼音,您可以根据自己的喜好选择安装fcitx-pinyin、fcitx-sogoupinyin、fcitx-sunpinyin. fcitx-table、fcitx-table-wubi、fcitx-table-wbpy
参考了Qlinux网友的内容,在这里表示感谢.
原文链接 ubuntu 14.04 使用极点五笔输入法
补充:

卸载ibus之后出现时间、声音、输入法、桌面等无法设置
原因:基础组件unity-control-center跟随ibus一起被卸载了
解决办法
sudo apt-get install unity-control-center
sudo apt-get install fcitx-table-wbpy

记得切换输入法的快捷键还是windows中习惯的Ctrl+Shift和Ctrl+Space(空格).
