---
layout: page
title: "Projects"
permalink: /Projects/
---

# List of current and past projects
{% assign ordered_projects = site.Projects | sort: 'page_order' %}
{% for project in ordered_projects %}
<h2><a href="{{project.url}}"> {{ project.title }}. </a> <img src="{{project.icon_img}}" style="width:15%; height:auto" class="icon_img"></h2>
{{ project.description }}
<br>
{{ project.dates }}
<hr>
{% endfor %}
