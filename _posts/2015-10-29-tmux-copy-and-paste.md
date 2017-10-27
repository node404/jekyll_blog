---
layout: post
title: "tmux的复制和黏贴"
description: ""
category:
tags: [linux, tmux]
---
{% include JB/setup %}

tmux确实是一个吊炸的东西，功能和那个和他类似的鼻祖screen相比，确实多出了不少。比如他又一个可以对终端的内容进行复制和黏贴的功能。
这里假定你设置的快捷键前缀是Ctrl+a:
进行复制的操作是：

1. ```Ctrl+a [ ```开始复制
2. 把光标移动到开始复制的位置(上下左右键，或者Ctrl+n/p/b/f键移动光标)
2. ```Ctrl+Space```  开始选择内容，类似vim的按下V的效果
3. 完成选择，```Alt+w``` 完成复制
4. 然后到你想要黏贴的地方， ```Ctrl+a ]``` 黏贴成功

需要注意的是， Ctrl+space在windows和linux的某些桌面环境下，都是输入法切换键，所以如果这个方法不成功，很可能是因为快捷键冲突的原因了。那么解决方法一个是把冲突的快捷键解决掉，另一个这是通过配置.tmux.conf。其实上面的快捷键默认的快捷键，模仿的按键是emacs，因为我不用emacs（因为我不用emacs，所以也只觉得很不习惯)，其实还有另外的一种快捷键模式是模仿vim的，不过需要你在tmux配置文件里配置一下, 把下面这行加入.tmux.conf里:

	set-window-option -g mode-keys vi

设置成这样以后，重新载入配置（可以用```ctrl+a :```然后输入```source-file ~/.tmux.conf```.

然后复制黏贴的操作就变成了：

1. ```Ctrl+a [ ```开始复制
2. 把光标移动到开始复制的位置(上下左右键，或者Ctrl+n/p/b/f键移动光标)
2. ```Space```  开始选择内容，类似vim的按下V的效果
3. 完成选择，```Enter``` 完成复制
4. 然后到你想要黏贴的地方， ```Ctrl+a ]``` 黏贴成功
