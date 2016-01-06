---
layout: post
title: "uwsgi start django"
description: ""
category:
tags: [python, uwsgi, django]
---
{% include JB/setup %}

没人访问的博客也不是不能升级。不过最近有点懒，趁着睡不着搞了下```uwsgi```。
额。平台ubuntu, 你最好有个django项目，假设已经用了```virtualenv```，假设已经安装好```nginx```并启动服务。

1）首先安装```uwsgi```:  

	pip install uwsgi


2）然后启动， 指定端口号和进程号：

	uwsgi -s 127.0.0.1:38803 --http :8000 --module blog.wsgi


3) 配置nginx配置文件，放在```/etc/nginx/site-enabled/```目录下，注意如果你要用80端口，就不要和其他的例如默认的配置冲突，如果有个default的文件，应该是一个软连接，可以直接删掉。我就直接贴我自己的了：


	upstream django {
  		server 127.0.0.1:38803;
	}

	server {
    	listen 80;
    	server_name localhost;
    	charset utf-8;
    	client_max_body_size 75M;
    	location / {
        	uwsgi_pass django;
        	include /etc/nginx/uwsgi_params;
    	}
    	location /static {
        	alias   /webapps/blog/static;
    	}
	}



4) 关于static，你应该在服务器某个地方建个目录(例如我的配置文件是```/webapps/blog/static```)，然后配置一下```setting.py```里的```STATIC_ROOT```成该目录，然后每次static有更改就重新执行

	python manage.py collectstatic.

然后尝试一下访问```127.0.0.1```，当然如果是远程服务器，就是你的网址了。
看看是否work了吧！
