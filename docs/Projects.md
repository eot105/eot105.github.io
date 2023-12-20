---
layout: page
title: "Projects"
permalink: /Projects/
---

# List of current and past projects

{% for project in site.Projects %}
<h2><a href="{{project.url}}"> {{ project.title }}. </a> <img src="{{project.icon_img}}" style="width:15%; height:auto" class="icon_img"></h2>
{{ project.description }}
<hr>
{% endfor %}
