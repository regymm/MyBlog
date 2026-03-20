---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: Welcome!
layout: page
---

<div class="home">
<img src="/MyBlog/images/aifreecontent.png" width="40%"/>
<h4>[2026-03-20]*This blog does not contain AI-generated contents(text, image, code, etc.) unless otherwise stated.  </h4>
<h4>[2022-09-18]Various website rearrangements </h4>
<h4>[2022-06-08]A lot of comic arrived recently! </h4>
<h4>[2021-08-27]RSS problem fixed: <a href="https://www.ustcpetergu.com/MyBlog/feed.xml">here</a>  </h4>
<h4>[2021-08-27]Added Friends column -- Email me to have you added! </h4>
<h4>[2021-02-03]Checkout Projects and Comics at upper-right corner, more coming! </h4>
</div>

<ul class="post-list">
{% for post in site.posts %} 
<li>
  {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
  <span class="artdate_main">{{ post.date | date: "%Y-%m-%d" }}</span>
  <br/>
  <a href="{{ site.baseurl }}{{ post.url }}" style="font-size:24px">{{ post.title }}</a>
  <!--<br/>-->
</li>
{% endfor %}
</ul>

<div style="font-size: 16px">
    <img src='https://licensebuttons.net/l/by-sa/4.0/88x31.png'/>
    This work is licensed under the <a href='https://creativecommons.org/licenses/by-sa/4.0/deed.en'>Creative Commons BY-SA 4.0 International License</a>, if not explicitly specified. 
</div>
