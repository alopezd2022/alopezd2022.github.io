---
layout: default
title: Robótica Móvil
permalink: /robotica-movil/
---
Welcome to the Mobile Robotics blog! In this blog, you will find content and solutions to the practices proposed by the Mobile Robotics course. Each practice will include a description, details about it, the hardware used, and finally, the software developed and implemented. Additionally, you will find a video that showcases the final operation and process, detailing errors and possible solutions until reaching the final solution.
<ul>
  {%- assign sorted_posts = site.posts | sort: "date" -%}
  {%- for post in sorted_posts -%}
    {%- if post.tags contains "robotica-movil" -%}
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



