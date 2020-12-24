---
layout: page
title: Projects
permalink: /projects/
---

<h2> Coming soon! </h2>
<!--<ul>-->
  {% for post in site.posts %}
	  {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
      <span style="color:#828282;font-size:14px">{{ post.date | date: date_format }}</span>
	  <br/>
      <a href="{{ post.url }}" style="font-size:24px">{{ post.title }}</a>
	  <br/>
	  <br/>
  {% endfor %}

<!--</ul>-->
