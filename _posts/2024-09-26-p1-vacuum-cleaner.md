---
layout: post
title:  "P1 Vacuum Cleaner"
---


## Description
The goal is to develop a software for a vacuum cleaner robot. The robot must explore the most surface of the house.
For that, we only have the laser sensor and the bumper; therefore the software for this application is not quite difficult.

Remember that the vacuum cleaner cannot make use of localization algorithms.



## Hardware

![Robot](/assets/images/Captura desde 2024-09-27 15-52-32.png)

* As actuators, we have the wheels of the robot, which can move in a linear and angular way.

* As sensors, we have a laser LIDAR 180º and a bumper.


## Software
The system must be reactive, so that I have developed a state machine whose behavior is described below.

![Diagrama de estados](/assets/images/p1-vacuum-cleaner.drawio.png)
