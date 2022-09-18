---
layout: page
title: Comics
permalink: /comic/
---

<h3> Here're my original webcomics </h3>
<!--<ul>-->
  {% for post in site.posts %} 
  {% if post.categories contains "Comic" %}
	  {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
      <span class="artdate">{{ post.date | date: "%Y-%m-%d" }}</span>
	  <!--<br/>-->
      <a href="{{ site.baseurl }}{{ post.url }}" style="font-size:24px">{{ post.title }}</a>
	  <!--<br/>-->
  {% endif %}
  {% endfor %}

<!--</ul>-->
