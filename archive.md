---
layout: page
title: Archive
---


{% for tag in site.tags %}
  {% assign t = tag | first %}
  {% assign posts = tag | last %}

<h3 class="category-key" id="{{ t | downcase }}">{{ t }}</h3>

<ul class="year">
  {% for post in posts %}
    {% if post.tags contains t %}
      <li>
      <span class="date">{{ post.date | date_to_string  }}</span>
      <a href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endif %}
  {% endfor %}
</ul>


{% endfor %}

<!-- 
{% for post in site.posts %}
* {{ post.date | date_to_string }} [ {{ post.title }} ]({{ post.url }})
{% endfor %}


 -->