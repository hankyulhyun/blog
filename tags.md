---
layout: page
title: Tags
# header: Posts By Tag
# group: navigation
# permalink: /tags
---


{% for tag in site.tags %}
<h2 id="{{ tag[0] }}-ref">{{ tag[0] }}</h2>

<ul>
{% for post in tag[1] %}
<li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
{% endfor %}
