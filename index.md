---
title: "bloğ"
---

## You're ready to go!

Start developing your Jekyll website.

<h1>{{ "Hello World!" | downcase }}</h1>

{% for tag in site.tags reversed%}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a>{{ post.excerpt }}</li>
    {% endfor %}
  </ul>
{% endfor %}