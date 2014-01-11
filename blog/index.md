---
layout: default
title: "@demisx&mdash;Born-again Rubyist Blog"
---

<div id="home">
  {% for post in site.posts limit: 5 %}
    <div class="row">
      <div><a href="{{ post.url }}"><h2>{{ post.title }} &raquo;</h2></a></div>
      <hr>
      <div><span class="small text-muted">{{ post.date | date: "%B %d, %Y" }}</span></div>
      <div>
        <p>
          {{ post.excerpt }} <a href="{{ post.url }}" class="lead">Read&nbsp;more</a>
        </p>
      </div>
    </div>
  {% endfor %}
</div>
