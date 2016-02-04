---
layout: post
title: "在Archlinux上安装mariadb, nginx and phpMyAdmin"
description: ""
category: 
tags: [nginx, mysql, php]
---
{% include JB/setup %}

phpMyAdmin, MariaDB, Nginx在archlinux 上的文档,

[Nginx](https://wiki.archlinux.org/index.php/nginx#PHP_implementation)

[MariaDB/MySQL](https://wiki.archlinux.org/index.php/MySQL)

[phpMyAdmin](https://wiki.archlinux.org/index.php/PhpMyAdmin#Apache)


1. 安装mariadb is easy, 分别mariadb即可。

    可以通过```mysql_secure_installation```命令配置root的用户名和密码, 如果不想用root可以建一个别的用户，赋给他某个数据库的权限


        mysql -u root -p
        MariaDB> CREATE USER 'monty'@'localhost' IDENTIFIED BY 'some_pass';
        MariaDB> GRANT ALL PRIVILEGES ON mydb.* TO 'monty'@'localhost';
        MariaDB> FLUSH PRIVILEGES;
        MariaDB> quit

    另外还可以配置一下编码和允许远程访问等，中文用户一般应该配置成utf-8，可以修改/etc/mysql.my.cnf或者~/.my.cnf具体进行配置.wiki上都有比较详细的解释

2. 安装nginx，用```systemctl start nginx```启动服务，看一下localhost是否能正常访问（确认有没有应用占用了80端口）

3. 安装php和php-fpm包，虽然完全不懂php，但是phpMyAdmin是我一直一来非常喜欢的工具，根据名字就知道这个是php写的，所以必须要装上php相关的东西

   安装完成后需要完成一些必要的操作。

   1) 编辑```/etc/php/php.ini```文件, 启用默认被注释掉的以下插件

        extension=mysqli.so
        extension=mcrypt.so
        extension=pdo_mysql.so

   2) 把```/etc/php/php.ini```的```open_basedir```的路径，改成以下路径：

        open_basedir = /srv/http/:/home/:/tmp/:/usr/share/pear/:/usr/share/webapps/:/etc/webapps/

   3) 启动php-fpm服务:

        systemctl start php-fpm

4. 配置```nginx```的```phpmyadmin```子域名, 我采用了subdomain的方法，wiki上还介绍了subdirectory的方法:

   在``/etc/nginx/nginx.conf```里，添加如下内容, <domain.tld>改成你的域名，如果是本地可以直接改成localhost，或者一个你不太会用到的网址：

        server {
              server_name     phpmyadmin.<domain.tld>;
      
              root    /usr/share/webapps/phpMyAdmin;
              index   index.php;
      
              location ~ \.php$ {
                      try_files      $uri =404;
                      fastcgi_pass   unix:/run/php-fpm/php-fpm.sock;
                      fastcgi_index  index.php;
                      include        fastcgi.conf;
              }
        }

5. 修改hosts文件```/etc/hosts```, 加入自定义的域名指向本地域名

        127.0.0.1	phpmyadmin.<domain.tld>

6. 这样应该是配置完成了，启动nginx服务```systemctl start nginx```，可以打开网址phpmyadmin.<domain.tld>测试一下。

Arch的文档非常完善，但是有时候并不针对入门用户，所以很有必要把几个文档一起关联一下。
比如这次我安装phpMyAdmin，通过该页面对nginx和php的配置都是一带而过，甚至都没有提到php-fpm，所以对php不是很熟的我一开始遇到了一些麻烦。不过即便如此，Arch的wiki依然是我见过最强大的wiki，在ubuntu上依然要碰到各个版本配置方法都不太相同的问题，在Archlinux上基本上不会有这样的坑。
扯得有点多了。最近博客有哦点懒得写。恢复两天一篇吧。
