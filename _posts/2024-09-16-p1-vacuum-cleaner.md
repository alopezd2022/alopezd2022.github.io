---
layout: post
title:  "P1 Vacuum Cleaner"
---


## Description
&nbsp;
The goal is to develop a software for a vacuum cleaner robot. The robot must explore the most surface of the house.
For that, we only have the laser sensor and the bumper; therefore the software for this application is not quite difficult.

Remember that the vacuum cleaner cannot make use of localization algorithms.

&nbsp;
## Hardware

<div style="text-align: center;">
    <img src="/assets/images/Captura desde 2024-09-27 15-52-32.png" alt="Aspiradora Robot">
</div>
&nbsp;
* As actuators, we have the wheels of the robot, which can move in a linear and angular way.

* As sensors, we have a laser LIDAR 180º and a bumper.

&nbsp;
## Software
&nbsp;
The system must be reactive, so that I have developed a state machine whose behavior is described below.
&nbsp;
<div style="text-align: center;">
    <img src="/assets/images/p1-vacuum-cleaner.drawio.png" alt="Aspiradora Robot">
</div>
&nbsp;
As you can see, my solution to the problem is a reactive and random algorithm, in which the movements in the "Turn" and "Forward" states are random. However, when the robot collides and activates the bumper state, it first moves slightly backward and then has two options. One is "Turn max angle", which, as the name suggests, uses the laser to find the direction with the greatest detected distance and turns towards it accordingly. The other option is "Turn half angle", where the robot rotates on its axis and changes direction by turning approximately 180º.

### Videos
&nbsp;
In this video, you can see the robot's operation. The video is at 4x speed, so it will appear accelerated.
&nbsp;
<div style="text-align: center;">
    <video width="600" controls>
      <source src="{{ '/assets/videos/Video-completo.webm' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>
&nbsp;
In this other video, you can see how the robot exits the room, avoiding getting trapped inside it.
<div style="text-align: center;">
    <video width="400" controls>
      <source src="{{ '/assets/videos/Video-habitación.webm' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>
&nbsp;

## Conclusion
At first, when the practice was presented to us, I thought about creating complex software using geometric shapes that would cover the largest possible area. However, in reality, developing complex software to perform these tasks did not seem appropriate for this practice, as we had to develop a reactive algorithm. Therefore, I ultimately opted for a random algorithm that provides random movements and is constantly checking the sensors, which is where the reactivity comes from. This resulted in a simple algorithm for a sensory cleaning robot.

