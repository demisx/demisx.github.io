---
layout: default
title: "@demisx&mdash;Born-again Rubyist Blog"
---

<div id="main-content">
  {% for post in site.posts limit: 10 %}
    <div class="row">
      <div><a href="{{ post.url }}"><h2>{{ post.title }}&nbsp;&raquo;</h2></a></div>
      <hr>
      <div><span class="small text-muted">{{ post.date | date: "%B %d, %Y" }}</span></div>
      <div>
        <p>
          {{ post.excerpt }} <a href="{{ post.url }}" class="lead">Read&nbsp;the&nbsp;entire&nbsp;post...</a>
        </p>
      </div>
    </div>
  {% endfor %}
</div>
