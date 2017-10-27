---
layout: post
title: "git restore file and revert commit"
description: ""
category: git
tags: [git]
---
{% include JB/setup %}

# restore file

check the commit ID:

    Git log
    commitID：abcde


Assuming the commit you want is abcde:

    git checkout abcde file/to/restore


# revert commit:

check the commit ID:

    git log

Reset commit_id ( 3 options : –soft –mixed –hard ):

    git reset --hard commit_id 

If you already commited, you should choose if push force or do something else:

    git push <HEAD> master --force


PS: 根据–soft –mixed –hard，会对working tree和index和HEAD进行重置:

    1. ```git reset –mixed:```此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息

    2. ```git reset –soft:```回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可

    3. ```git reset –hard:```彻底回退到某个版本，本地的源码也会变为上一个版本的内容


---

[version control - Reset or revert a specific file to a specific revision using Git? - Stack Overflow](http://stackoverflow.com/questions/215718/reset-or-revert-a-specific-file-to-a-specific-revision-using-git)
[git撤销commit](http://zhyq0826.iteye.com/blog/1671638)
