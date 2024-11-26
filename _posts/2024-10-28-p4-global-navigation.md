---
layout: post
title:  "P4 Global Navigation"
tags: [robotica-movil]
---

## Description

The objective of this practice is to develop a global navigation algorithm. The user marks a point on the map, and the car must reach it by the shortest path.


## Hardware

We only have a car without any sensors. The only information we have is its current position.

## Software

### Wave Front Algorthim
For the car's navigation, we developed a search algorithm based on the Wave Front Algorithm. This approach generates a cost map by expanding from the final target, assigned a cost of 0, to the car, which receives a cost of *n*.

To create the cost map, successors of each point are assigned a value equal to *cost + 1*, while diagonal successors are assigned a value of *cost + 1.41* (representing the Euclidean distance).

<div style="text-align: center;">
    <img src="/assets/images/p4/Captura desde 2024-11-18 19-01-14.png" alt="car" style= "width: 300px">
</div>

Additionally, obstacles and positions near them are assigned an extra cost to ensure that these points are avoided during navigation.


Once the search and the filling of the cost grid are completed, we display the map based on the cost grid. However, since the costs exceed 255 (the maximum white color), we need to normalize it so that it is displayed in a range from 0 to 255 based on the highest cost.

<div style="text-align: center;">
    <img src="/assets/images/p4/Captura desde 2024-11-26 11-44-16.png" alt="car" style= "width: 300px">
</div>

### Gradient algorthim

Once the cost map is generated, we implement a gradient-based navigation algorithm. It works using potential fields: obstacles act as "potential walls" that the robot avoids, while the target serves as a "potential well" that attracts it. By combining these walls and wells, a downward slope path is created, and the robot simply follows it to reach its destination.


&nbsp;

### Videos


<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p4/grabacion-de-pantalla-desde-2024-11-18-10-37-27_hUPDu7c6.webm' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>

&nbsp;

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p4/grabacion-de-pantalla-desde-2024-11-18-10-44-35_0AQSLjZU.webm' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>

