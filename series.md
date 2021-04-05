---
title: series
---
<h1>series</h1>

<ul>
  {% for serie in site.series %}
    <li>
        <h2><a href="{{ serie.url }}">{{ serie.title }}</a></h2>
        <p>{{ serie.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>