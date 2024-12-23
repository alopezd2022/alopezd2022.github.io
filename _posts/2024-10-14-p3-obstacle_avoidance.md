---
layout: post
title:  "P3 Obstacle Avoidance"
tags: [mobile-robotics]
---

## Description

The aim of this practice is to program a Formula 1 car which can avoid obstacle using Virtual Force Field system. In this case, we are going to develop a local navigating algorthims so that the car can move throw the circuit reaching the waypoints and avoiding the obstacles.

<div style="text-align: center;">
    <img src="/assets/images/p3/coche.png" alt="car" style= "width: 300px">
</div>

## Hardware

The sensor is the laser LIDAR 180º and the actuators are as always the motor of the Formula 1 car.

## Software

The strategy for this practice is develop a local navegation algorthim called VFF (Virtual Force Field). Is a navegation tecnice which allows our formula 1 car to move in a environment avoiding obstacles. Basically, the direction of the car is given by the attractive force generated by the position of the goal.

### VFF
The attractive vector is the vector drawn from the car's position to the waypoint. This vector becomes very strong over long distances, so we will apply a force reducer.

The repulsive vector is the result of a weighted average of all the vectors drawn for all the angles of the laser. It is a weighted average because, in certain situations, such as very short distances, they do not have enough weight, and therefore the repulsive vector is not effective.

The resulting force should be the weighted average of both the attractive and repulsive vectors. Depending on the factors we assign to each, one vector will have more weight than the other. In this case, I found it more convenient to give greater weight to the repulsive vector so that it prioritizes avoiding obstacles.


#### Strategies to highlight

Reduce the size of the attractive vector, as at very high distances, it exerts too much force.
    
Increase the weight of small distances measured by the laser, so that when calculating the repulsive vector, these distances have more significance.
    
The linear velocity depends on the distance calculated as the magnitude of the resulting vector from the weighted average of both forces. This way, at short distances, the speed is reduced to allow for precise evasion.

&nbsp;

Here it is a video with the behavior of the formula 1 car.
    
<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3/cochevueltacompleta.webm' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>

