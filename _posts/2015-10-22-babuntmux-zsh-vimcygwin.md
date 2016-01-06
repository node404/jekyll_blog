---
layout: post
title: "利用babun, tmux, zsh高度定制shell"
description: ""
category:
tags: [shell, tmux, zsh]
---
{% include JB/setup %}

深刻和彻底的切换到linux平台后，深切的感受到linux平台对程序员带来的便利。因此日常环境已经不再使用windows啦，奈何博主虽然是个穷逼，然而却舍得钱买一堆电脑，所以没事还是会折腾下windows，毕竟娱乐还是需要的。

之前折腾过cygwin好一阵子，最终还是觉得真的搬到开发环境确实有很多麻烦的地方，不过现在既然为了娱乐又有一台没有装任何开发环境的win10，那就娱乐一下好了。

# 1） 安装和配置babun

首先可以装一个定制得非常好的cygwin shell, 那就是[babun](http://babun.github.io/), 已经默认为你装好并配置了cygwin环境下的zsh，用起来可比普通的shell爽多了。另外有个强大的包管理器，pact命令，用起来很像rpm和apt之类的，非常方便。例如要装tmux, 那么就执行

	pact install tmux

就可以成功安装了。
不过值得注意的是pact默认用的是kernel.org的镜像，国内访问非常慢，这时通过help发现有个--mirror的参数，可以手动指定成163的：

	pact --mirror http://mirrors.163.com/cygwin/ install tree

不过每次要输入这个太麻烦，可以用alias, 在home目录下新建一个.bash_aliases的文件，输入alias pact='pact --mirror http://mirrors.163.com/cygwin/'这样下次就不需要再输--mirror了。

# 2） 安装和配置tmux

OK第一步装babun已经完成，接下来装tmux，也非常easy，其实上面已经介绍过了

	pact install tmux

然后就可以用了，输入tmux, 下面就有了一个标签栏，tmux默认的前缀按键是Ctrl+B，然后再按另一个键，随便举几个例子：

1. Ctrl+b c 创建标签
2. Ctrl+b n 下一个标签
3. Ctrl+b p 上一个标签
4. Ctrl+b d 分离session，但并没有退出，而是在后台运行，如果要重新链接，输入tmux a
5. Ctrl+b ? 查看帮助

tmux非常强大，甚至有分屏这样的操作，具体可以在帮助里看。重要的是我们现在已经可以只打开一个窗口，就在几个window之前切换了。

OK, 看起来非常不错，但是用过vim的同学知道，Ctrl+b是向下翻页的按键啊，所以我想把前缀按键换成Ctrl+a怎么办, 还是配置文件，linux的一切都是配置文件。
在Home目录创建一个.tmux.conf文件，输入以下内容：

	unbind C-b
	set -g prefix C-a
	bind C-a send-prefix

如果你的tmux已经打开，你想重载配置，怎么操作呢? 前缀Ctrl+b ：然后标签栏就会出现一个冒号，输入

	source-file ~/.tmux.conf

回车，好了，此时前缀应该就已经变成了Ctrl+a了
