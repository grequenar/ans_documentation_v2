---
layout: page
title: ans_ibe_linux
group: linux
permalink: /linux/
---
{% assign posts = site.posts | where:"group", "linux" %}

{% for post in posts %}
<li>
<span class="date">{{ post.date | date: '%B %d, %Y' }}</span>
<h3>
<a class="post-link" href="{{ site.url }}{{site.baseurl}}{{ post.url }}">{{ post.title }}</a>
</h3>
</li>
{% endfor %}


