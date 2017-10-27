---
layout: post
title: "git rm introduce "
description: ""
category: git
tags: [git]
---
{% include JB/setup %}

在git中我们可以通过git rm命令把一个文件删除，并把它从git的仓库管理系统中移除。但是注意最后要执行git commit才真正提交到git仓库
示例1


	git rm 1.txt

删除1.txt文件，并把它从git的仓库管理系统中移除。

示例2


	git rm -r myFolder


删除文件夹myFolder，并把它从git的仓库管理系统中移除。

```git add -i```  can check which file can be git add.

示例3


	$ git add 10.txt
	$ git add -i
           	staged     unstaged path
  	1:        +0/-0      nothing 10.txt
  	2:        +0/-0      nothing branch/t.txt
 	3:        +0/-0      nothing branch/t2.txt
	*** Commands ***
	  1: [s]tatus     2: [u]pdate     3: [r]evert     4: [a]dd untracked
	  5: [p]atch      6: [d]iff       7: [q]uit       8: [h]elp
	What now> 7
	Bye.
	$ git rm --cached 10.txt
	rm '10.txt'
	$ ls
	10.txt  2  3.txt  5.txt  readme.txt
	$ git add -i
	           staged     unstaged path
	  1:        +0/-0      nothing branch/t.txt
	  2:        +0/-0      nothing branch/t2.txt
	*** Commands ***
	  1: [s]tatus     2: [u]pdate     3: [r]evert     4: [a]dd untracked
	  5: [p]atch      6: [d]iff       7: [q]uit       8: [h]elp
	What now>


在通过 git add 10.txt 命令把文件10,txt添加到索引库中后，又通过 git rm --cached 10.txt 把文件10.txt从git的索引库中移除,但是对文件10.txt本身并不进行任何操作。
ra
另外对于已经被git rm删除掉（还没被提交）的文件或目录，如果想取消其操作的话，可以首先通过git add -i的子命令revert从索引库中把它们剔除，然后用git checkout <文件>命令来达到取消的目录

关于git add请参考《git add详解》
本文翻译整理自：[http://web.mit.edu/~mkgray/project/silk/root/afs/sipb/project/git/git-doc/git-rm.html](http://web.mit.edu/~mkgray/project/silk/root/afs/sipb/project/git/git-doc/git-rm.html)