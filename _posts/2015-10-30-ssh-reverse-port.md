---
layout: post
title: "ssh反向端口转发"
description: ""
category:
tags: [linux, ssh]
---
{% include JB/setup %}
从本地Host用SSH连接到目标主机，怎么让目标主机也可以连接本地的SSH呢？
答案很简单，只要把本地主机的SSH端口转发到目标主机即可。可以先查看一下SSH命令的man手册:

	man ssh

注意里面的R参数，就是要用到的参数：

	-R [bind_address:]port:host:hostport
             Specifies that the given port on the remote (server) host is to be forwarded to the given host and port on the local side.  This works by allocating a socket to listen to port

意思就是把一个远程主机的端口用于本地端口的端口转发。于是对应于假设远程主机IP:192.168.172.11, 本地ssh端口是默认的22，我们可以这样做：

	ssh -R 17999:localhost:22 sourceuser@192.168.172.11

这一步完成以后，远程的17999端口会映射到本地主机的22端口，于是我们在远程终端，输入命令:

	ssh -p 17999 127.0.0.1

这样就很方便的又重新连接到了本地的ssh。同样的技巧可以用于一些暴露本地端口的操作，例如你身处一个局域网里，且没有权限去操作路由。你却想要暴露一个本机的ip，比如说80，让远程的主机访问你的http服务，用同样的方式可以很轻易的做到。
