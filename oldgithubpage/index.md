---
layout: page
title: Try 
tagline: try is always better than not 
---
{% include JB/setup %}



I'm a  Java/Python developer. I'm starting blog and collect posts in this blog. It will mainly focused on  Linux, Java, Python, Git and etc... 

So maybe I won't share my personal life here  :-)

Most of there posts are collected from Internet, I will try to write every source URL at the end of every article, but I can't cover all. Please let me know if there is any copyright issues.

The source code of this blog is on github: please check 
my github page: [realhu1989/realhu1989.github.io](https://github.com/realhu1989/realhu1989.github.io) and feel free to download them or commit any pull requests.



---

- My Personal Email: mayhyy@gmail.com

- My Posts List:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


---
This blog is powered by Jekyll, Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)

Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)