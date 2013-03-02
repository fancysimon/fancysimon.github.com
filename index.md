---
layout: page
title: Hello World!
tagline: Supporting tagline
---
{% include JB/setup %}

{% for post in site.posts %}
    [{{ post.date | date_to_string }}]({{ BASE_PATH }}{{ post.url }}{{ post.title }})
{% endfor %}

