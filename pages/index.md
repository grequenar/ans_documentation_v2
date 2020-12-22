---
layout: wrapper
title: Home Page
permalink: /index.html
---

<h2 class="group-list-heading">Ansible Iberia Groups</h2>
<ul class="group-list">

{% for object in site.data.ans_doc.ansible_objects %}
<li>
<h3>
{% assign listAtributesName = object.name | split:"_"  %}
<a class="group-link" href="{{ site.url }}{{site.baseurl}}/{{listAtributesName[0]}}/{{listAtributesName[1]}}/{{listAtributesName[2]}}/{{listAtributesName[3]}}/{{object.name}}.html">
	{{ object.title }} <br>
</a>
</h3>
</li>
{% endfor %}

</ul>


