---
layout: wrapper
title: Home Page
permalink: /index_improved.html
---

<h2 class="group-list-heading">Ansible Iberia Groups</h2>
<ul class="group-list">
<div id="menu">
{% assign padre = "" | split: ',' %}
{% assign padre1 = "" | split: ',' %}
{% assign padre2 = "" | split: ',' %}
{% assign padre3 = "" | split: ',' %}
{% assign padre4 = "" | split: ',' %}
{% for object in site.data.ans_doc.ansible_objects %}
	{% assign listAtributesName = object.name | split:"_"  %}
	{% unless padre contains listAtributesName[0] %}
		{% assign padre = padre | push: listAtributesName[0] %}
		<ul>
		<li class="has-sub"><a title="{{listAtributesName[0]}}">{{listAtributesName[0]}}</a>
		
		{% for object1 in site.data.ans_doc.ansible_objects %}
			{% assign listAtributesName1 = object1.name | split:"_"  %}
			{% assign check1 = listAtributesName1[0] | append: listAtributesName1[1] %}
			{% unless padre1 contains check1 %}
				{% assign padre1 = padre1 | push: check1 %}
				<ul>
				<li class="has-sub"><a title="{{listAtributesName1[1]}}">{{listAtributesName1[1]}}</a>

				{% for object2 in site.data.ans_doc.ansible_objects %}
					{% assign listAtributesName2 = object2.name | split:"_"  %}
					{% assign check2 = listAtributesName2[0] | append: listAtributesName2[1] | append: listAtributesName2[2] %}
					
					{% if listAtributesName2[0] == listAtributesName[0] and listAtributesName2[1] == listAtributesName1[1] %}
					{% unless padre2 contains check2 %}
						{% assign padre2 = padre2 | push: check2 %}
						<ul>
						<li class="has-sub"><a title="{{listAtributesName2[2]}}">{{listAtributesName2[2]}}</a>

						{% for object3 in site.data.ans_doc.ansible_objects %}
							{% assign listAtributesName3 = object3.name | split:"_"  %}
                                        		{% assign check3 = listAtributesName3[0] | append: listAtributesName3[1] | append: listAtributesName3[2] | append: listAtributesName3[3] %}
                                        		{% if listAtributesName3[0] == listAtributesName[0] and listAtributesName3[1] == listAtributesName1[1] and listAtributesName3[2] == listAtributesName2[2] %}
							{% unless padre3 contains check3 %}
                                                		{% assign padre3 = padre3 | push: check3 %}
								<ul>
								<li class="has-sub"><a title="{{listAtributesName3[3]}}">{{listAtributesName3[3]}}</a>
								
								{% for object4 in site.data.ans_doc.ansible_objects %}
									{% assign listAtributesName4 = object4.name | split:"_"  %}
                                        				{% assign check4 = listAtributesName4[0] | append: listAtributesName4[1] | append: listAtributesName4[2] | append: listAtributesName4[3] | append: listAtributesName4[4] %}
									{% if listAtributesName4[0] == listAtributesName[0] and listAtributesName4[1] == listAtributesName1[1] and listAtributesName4[2] == listAtributesName2[2] and listAtributesName4[3] == listAtributesName3[3] %}
									{% unless padre4 contains check4 %}
										{% assign padre4 = padre4 | push: check4 %}
						        			<ul>
						        			{% assign atributo_title = object4.title | split:"_"  %}
						        			<li><a title="{{ atributo_title }}" href="{{ site.url }}{{site.baseurl}}/{{listAtributesName4[0]}}/{{listAtributesName4[1]}}/{{listAtributesName4[2]}}/{{listAtributesName4[3]}}/{{object4.name}}.html">{{ atributo_title }}</a>
										</li>
						        			</ul>
									 {% endunless %}
									{% endif %}
								{% endfor %}
								</li>
								</ul>
						 	{% endunless %}
							{% endif %}
						{% endfor %}
						
						</li>
						</ul>
					{% endunless %}
					{% endif %}
				{% endfor %}

				</li>
				</ul>
			{% endunless %}
		{% endfor %}
		</li>
		</ul>
	{% endunless %}
{% endfor %}
</div>
</ul>




