---
layout: default
title: "@demisx&mdash;Software patterns and solutions blog"
---
<!--
<div id="main-content">
  {% for post in site.posts limit: 15 %}
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
</div> -->


<div id="main-content">
  {% for post in site.posts limit: 20 %}
  <div class="panel panel-default">
    <div class="panel-heading">
      <div class="post-date">
        <span class="text-muted">
          {{ post.date | date: "%B %d, %Y" }}
        </span>
      </div>
      <div class="post-title">
        <a href="{{ post.url }}">
          <h1>{{ post.title }}&nbsp;&raquo;</h1>
        </a>
      </div>
    </div>
    <div class="panel-body">
      <div>
        <p>
          {{ post.excerpt }} <a href="{{ post.url }}" class="lead">Read&nbsp;the&nbsp;entire&nbsp;post...</a>
        </p>
      </div>
    </div>
  </div>
  {% endfor %}
</div>
