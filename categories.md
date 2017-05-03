---
layout: page
title: Categories
# header: Posts By Category
# group: navigation
# permalink: /categories
---


{% for category in site.categories %}
<h2 id="{{ category[0] }}-ref">{{ category[0] | join: "/" }}</h2>

<ul>
{% for post in category[1] %}
<li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
{% endfor %}
