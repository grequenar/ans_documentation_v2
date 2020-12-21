---
layout: page
title: ans_ibe_windows
permalink: /windows/
---
{% assign posts = site.posts | where:"group", "windows" %}

{% for post in posts %}
<li>
<span class="date">{{ post.date | date: '%B %d, %Y' }}</span>
<h3>
<a class="post-link" href="{{ site.url }}{{site.baseurl}}{{ post.url }}">{{ post.title }}</a>
</h3>
</li>
{% endfor %}
