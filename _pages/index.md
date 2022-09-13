---
layout: defaults/page
permalink: index.html
narrow: true
title: Welcome to Friday Theme
---

## What is this?

{% include components/intro.md %}

<hr />

### Recent Posts

<hr />

{% for post in site.posts limit:3 %}
{% include components/post-card.html %}
{% endfor %}


