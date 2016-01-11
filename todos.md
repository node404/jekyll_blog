---
layout: page
title: Hello World!
tagline: Supporting tagline
---
{% include JB/setup %}


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

===

TODOS:

1）样式继续优化
2）把以前的一些分散的资料页面合并过来
3）把印象笔记的内容合并过来
