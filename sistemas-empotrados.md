---
layout: default
title: Sistemas Empotrados y de Tiempo Real
permalink: /sistemas-empotrados-tiempo-real/
---

<ul>
  {%- assign sorted_posts = site.posts | sort: "date" -%}
  {%- for post in sorted_posts -%}
    {%- if post.tags contains "sistemas-empotrados" -%}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
      </li>
    {%- endif -%}
  {%- endfor -%}
</ul>


