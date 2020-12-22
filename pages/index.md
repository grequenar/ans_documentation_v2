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
<a class="group-link" href="{{ site.url }}{{site.baseurl}}{{ object.name }}">
	{{ object.name }} <br>
</a>
</h3>
</li>
{% endfor %}

</ul>


