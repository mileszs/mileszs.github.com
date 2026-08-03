---
layout: page
title: "Archive"
permalink: /archive/
description: "Every post on mileszs.com, grouped by year — roughly two decades of writing on software, engineering leadership, books, and tabletop games."
comments: false
sharing: false
footer: false
---

<div class="archive">
  <h1 class="archive-heading">Archive</h1>

  <p class="archive-intro">
    Every post, newest first &mdash; {{ site.posts | size }} of them, going back to
    {{ site.posts.last.date | date: "%B %Y" }}. Also available as an
    <a href="{{ "/feed.xml" | relative_url }}">Atom feed</a>.
  </p>

  {% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
  {% for year in posts_by_year %}
    <h2 class="archive-year" id="{{ year.name }}">{{ year.name }}</h2>
    <ul class="archive-list">
      {% for post in year.items %}
        <li>
          <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b&nbsp;%d" }}</time>
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </li>
      {% endfor %}
    </ul>
  {% endfor %}
</div>
