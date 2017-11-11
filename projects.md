---
layout: page
title: Projects
---
{% assign t = "Projects" %}
<h3 class="category-key" id="{{ t | downcase }}">{{ t }}</h3>
{% for post in site.posts %}

<ul class="year">
    {% if post.tags contains t %}
      <li>
      <span class="date">{{ post.date | date_to_string  }}</span>
      <a href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endif %}

</ul>
  {% endfor %}