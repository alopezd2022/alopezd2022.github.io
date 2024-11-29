---
layout: default
title: Embedded and real-time systems
permalink: /embedded-and-real-time-systems/
---
Welcome to the Embedded and real-time systems blog! In this blog, you will find content and solutions to the practices proposed by the course. Each practice will include a description, details about it, the hardware used, and finally, the software developed and implemented. Additionally, you will find a video that showcases the final operation and process, detailing errors and possible solutions until reaching the final solution.

### **Objectives of the Course: Embedded and Real-Time Systems**  

In this section of the blog, we will explore key practices and concepts in **Embedded and Real-Time Systems**, focusing on the following main objectives:

- **Analyze and plan real-time systems:** develop the ability to understand the requirements and constraints of systems that must respond within specific timeframes.

- **Learn and program embedded systems:** design and program embedded devices that interact with their environment efficiently. 

- **Knowledge and programming of microcontrollers:** work with microcontrollers, understanding their architecture and applying low-level programming. 

- **Use of the Arduino platform (Atmega):** implement projects and practices using Arduino as the main platform, leveraging the capabilities of the Atmega microcontroller. 

- **Cross-compilation:** learn to compile and transfer software from a development machine to an embedded system.

## Practices

<ul>
  {%- assign sorted_posts = site.posts | sort: "date" -%}
  {%- for post in sorted_posts -%}
    {%- if post.tags contains "embedded-and-real-time-systems" -%}
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


