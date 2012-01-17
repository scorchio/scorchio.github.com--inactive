---
title: Archives
layout: base
---
<div class="archives">
  <div class="index">
    <h2><em>Archives</em></h2>
    <ul>
      {% for post in site.posts %}
        <li>
          <a href="{{ post.url }}" title="{{ post.title }}">
            <span class="date">
              <span class="day">{{ post.date | date: '%d' }}</span>
              <span class="month"><abbr>{{ post.date | date: '%b' }}</abbr></span>
              <span class="year">{{ post.date | date: '%Y' }}</span>
            </span>
            <span class="title">{{ post.title }}</span>
          </a>
        </li>
      {% endfor %}
    </ul>
  </div> <!-- /.index -->
</div>
