---
layout: post
title: "raspberry pi install dnscrypt"
description: ""
category: 
tags: []
---
{% include JB/setup %}

为什么就不言而喻不加解释了，就算你没有翻墙的打算，也建议你有一个优秀和安全的dns，因为国内的运营商除了干掉国外域名，还会帮你的国内网址投广告，甚至利用你进行ddns攻击。。

很早以前dnscrypt这个东西就在热炒,后来不知道什么原因谈起的人也比较少了。因此我决定重新尝试一下是不是还有用呢。答案是肯定的。不过如果你只是在本机上使用，那么如果是桌面系统，似乎我有看到dnscrypt是有windows/macOS的客户端的，但是手机怎么办呢。这个时候就是树霉派发挥作用的时候了。

树梅派安装dnscrypt相对与ubuntu之类的发行版其实也大同小异（我也没有试过ubuntu，arch上倒是非常简单，两三个命令就可以搞定），不过有些依赖包包括dnscrypt都是需要手动编译的。

1. 安装编译工具：

    sudo apt-get install libtool pkg-config build-essential autoconf automake

2. 安装一个叫做libsodium的依赖，因为apt工具里没有包含这个依赖，因此需要手动编译，去https://download.libsodium.org/libsodium/releases找最新的版本，下载并且编译：

    mkdir build
    cd build/
    wget https://download.libsodium.org/libsodium/releases/libsodium-1.0.3.tar.gz
    tar -zxvf libsodium-1.0.3.tar.gz
    cd libsodium-1.0.3/
    ./configure
    make
    sudo make install

3. 下载dnscrypt编译：
    
    wget https://github.com/jedisct1/dnscrypt-proxy/releases/download/1.6.1/dnscrypt-proxy-1.6.1.tar.bz2
    tar -jxvf dnscrypt-proxy-1.6.1.tar.bz2
    cd dnscrypt-proxy-1.6.1
    ./configure
    make -j2
    make install

4.  
