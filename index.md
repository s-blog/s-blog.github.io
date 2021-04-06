---
title: "bloğ"
---
The goal of this journal is to highlight and improve my process. Each series will explore how to integrate some popular libraries in boilerplate. Enjoy these fun learning exercises.

Good luck building!
<br/>
<br/>

<ul>
  {% for serie in site.series %}
    <li>
        <a href="{{ serie.url }}">{{ serie.title }}</a>
        <div class="seriePreview">{{ serie.content | markdownify }}</div>
        <br/>
    </li>
  {% endfor %}
</ul>

