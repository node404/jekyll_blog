---
layout: post
title: "git ignore files"
description: ""
category: git
tags: [git]
---
{% include JB/setup %}

有三种方法可以忽略掉这些文件，这三种方法都能达到目的，只不过适用情景不一样。


####(1). 针对单一工程排除文件，这种方式会让这个工程的所有修改者在克隆代码的同时，也能克隆到过滤规则，而不用自己再写一份，这就能保证所有修改者应用的都是同一份规则，而不是张三自己有一套过滤规则，李四又使用另一套过滤规则，个人比较喜欢这个。配置步骤如下：

>在工程根目录下建立.gitignore文件，将要排除的文件或目录写到.gitignore这个文件中，有两种写入方法。

(a)使用命令行增加排除文件
排除以.class结尾的文件 

	echo “*.class” >.gitignore 

排除bin目录下的文件 
	
	echo “bin/” >.gitignore
> (>> 是在文件尾增加,> 是删除已经存在的内容再增加)

(b)最方便的办法是，用记事本打开，增加需要排除的文件或目录，一行增加一个，如：

	*.class
	*.apk
	bin/
	gen/
	.settings/
	proguard/

####(2).全局设置排除文件，这会在全局起作用，只要是Git管理的工程，在提交时都会自动排除不在控制范围内的文件或目录。这种方法对开发者来说，比较省事，只要一次全局配置，不用每次建立工程都要配置一遍过滤规则。但是这不保证其他的开发者在克隆你的代码后，他们那边的规则跟你的是一样的，这就带来了代码提交过程中的各种冲突问题。配置步骤如下：


(a）像方法（1）一样，也需要建立一个.gitignore文件，把要排除的文件写进去。

(b) 但在这里，我们不规定一定要把.gitnore文件放到某个工程下面，而是任何地方，比如我们这里放到了Git默认的Home路径下，在我的windows上就是C:\Users\zhbpeng

(c)使用命令方式可以配置全局排除文件 

	git config --global core.excludesfile ~/.gitignore

你会发现在~/.gitconfig文件中会出现excludesfile = c:/Users/zhbpeng/.gitignore。说明Git把文件过滤规则应用到了Global的规则中。


####(3). 单个工程设置排除文件，在工程目录下找到.git/info/exclude，把要排除的文件写进去：

	*.class
	*.apk
	bin/
	gen/
	.settings/
	proguard/

> 这种方法就不提倡了，只能针对单一工程配置，而且还不能将过滤规则同步到其他开发者，跟方法（1）（2）比较起来没有一点优势。


Original Page: http://blog.sina.com.cn/s/blog_5ff02a9f0101lijs.html
Shared from Pocket