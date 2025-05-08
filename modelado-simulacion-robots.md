---
layout: default
title: Modelado y simulación de robots
permalink: /modelado-y-simulacion-de-robots/
---
Welcome to the Mobile Robotics blog! In this blog, you will find content and solutions to the practices proposed by the Mobile Robotics course. Each practice will include a description, details about it, the hardware used, and finally, the software developed and implemented. Additionally, you will find a video that showcases the final operation and process, detailing errors and possible solutions until reaching the final solution.

The course focuses on solving common problems in autonomous robots using advanced **algorithms and techniques** such as local and global navigation, mapping, self-localization, and **SLAM**. It includes hands-on practice in programming skills for mobile robots (drones, autonomous vehicles, etc.) using tools like **ROS** and **Gazebo**. It also covers swarm robot coordination and the use of advanced sensors. Prior knowledge of programming and robotics-related courses is recommended.

## Practices

<ul>
  {%- assign sorted_posts = site.posts | sort: "date" -%}
  {%- for post in sorted_posts -%}
    {%- if post.tags contains "modelado-y-simulacion-de-robots" -%}
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
