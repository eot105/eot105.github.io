---
layout: page
title: "Projects"
permalink: /Projects/
---

# List of current and past projects

{% for project in site.Projects %}
<h2><a href="{{project.url}}"> {{ project.title }}: </a></h2>

Design and build of a homemade 300J solid state flashlamp pumped Nd:Glass laser.
<hr>
{% endfor %}
