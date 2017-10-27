---
title: vim插件command-t在windows上编译
date: 2017-10-24
---

用Bundle在Windows平台上配置成功了以后，绝大多数插件都已经可以使用，但是唯独[Command-T](https://github.com/wincent/Command-T)因为是Ruby集成的插件，需要手动编译。

[官方文档](https://github.com/wincent/command-t/blob/master/doc/command-t.txt)写的安装编译过程代码大概是这样的，我省去了使用bundle的安装过程：

    $ cd ~/vim\bundle\command-t\ruby\command-t\ext\command-t>
    $ ruby extconf.rb
    $ make

要完成这些肯定得装ruby和make，网上的绝大多数答案也都已经过期，说的是vim7.4左右的版本，而现在已经是vim 8.0。于是只好自己探索。

windows的ruby是一个特殊的安装包，和Ruby的官网都分开，网址是[RubyInstaller for windows](https://rubyinstaller.org/)。

可供下载的版本从2.0.0-p648到2.4.2-2版本。

我从高版本开始尝试，安装2.4.2-2，安装成功并设置好windows环境变量，开始编译。结果发现报错 `You have to install development tools first.`. 于是我在官网发现了`DEVELOPMENT KIT`可以下载，但是居然只支持（Ruby 2.0 到 2.3），只好重新安装了Ruby 2.3, 并且按照官网development tools。

假设Ruby 安装到 C:\Ruby22-x64， 配置PATH环境变量。然后将Development tools 就解压到 C:\Ruby22-x64\DevKit, 然后进入该目录执行

    $ ruby dk.rb init

成功后再执行

    $ ruby dk.rb install

接着进入`~/vim\bundle\command-t\ruby\command-t\ext\command-t>`文件夹，执行`ruby extconf.rb`命令，发现ruby执行成功了，用choco安装make并执行make一切都顺利。
但是进入vim一按command-t的快捷键报错，报错内容是：

    command-t.vim could not load the C extension
    Please see INSTALLATION and TROUBLE-SHOOTING in the help
    Vim Ruby version: 2.2.6
    Expected version: 2.3.3

也就是vim内置的ruby版本和这个版本不匹配，看来我确实遗漏了vim内置版本的问题。于是继续降级到Ruby 2.2, 结果居然还是报错。报错内容依旧是`You have to install development tools first.` 然而我明明已经安装了development tools. 

求助google，结果在[RubyInstaller wiki](https://github.com/OneClick/RubyInstaller/wiki/Development-Kit)
有一段话是：

> The Hacky Developer Scenario – a developer building native gems wants to be able to quickly test that their extconf.rb file used to create a Makefile for the native library works correctly. To shorten the development cycle, the DevKit enables the developer to run `ruby -rdevkit extconf.rb`.


于是我尝试带`-rdevkit`参数执行extconf.rb，结果果然通过了，但是make命令却无法通过。继续搜索，发现[AskUbuntu]上有个command-t的问题，没有用官方文档的方法编译command-t而是用了`rake make`来编译，我知道rake是ruby的一个工具，在jekyll尝试搭博客的时候用到，但不知道原理。但是反正是编译只要过了就行，于是我就尝试执行`rake make`结果还是不信，报错`You have to install development tools first.`，但是这样我没有理由不去尝试帮`rake make` 带上`-rdevkit`这样一个参数，结果居然真的成功了。

于是迫不及待打开vim，按下command-t的快捷键，一个漂亮的弹窗弹了出来。试着敲了几行，好像没有什么问题，大功告成。

![command-t on windows]({{ "/assets/images/commandt1.png" | absolute_url }})
