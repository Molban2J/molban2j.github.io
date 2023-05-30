---
title: "Project"
layout: archive
permalink: /project
---


{% assign posts = site.categories.project %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}