---
layout: post
title:  "Logistic robot"
tags: [service-robotics]
---

## <b>Description</b>

This practice consists of programming a **logistic robot** that holds the shelf and move to a determinate point at a warehouse.

For this purpose, **OMPL** is used to get the path for each point and a reactive control is used to move along the warehouse.

In this practice there are two different robots, **holonomic** and **Ackermann**. The first solution to this pratice is using the **holonomic robot** and applying **differents geometries** to validate the path. The second solution is using an **Ackermann robot** and applying **differents geometries** to validate the path.

## <b>Strategy</b>

The overall operation of the system is based on a **state machine** that changes dynamically:

<div style="text-align: center;">
    <img src="/assets/images/p4_servicios/maquinaestados.png" alt="sin erosión" style="width: 300px; display: inline-block; margin: 0 10px;">
</div>

### <b>1º State: START </b>

In this phase, the global variable are initialize and the execution starts.

### <b>2º State: PLANNING </b>

In this phase, get the actual pose of the robot in order to use it as the start point for the OMPL algorithm. 
If a plan is found, switch to the **GOTO** state.

### <b>3º State: GOTO </b>

In this phase, the robot is controlled to reach a certain point using the function **move_control** which implements a P control. When a point is reached by the robot, the state switches to **CHANGE_TARGET** in which the target of the path is changed or if the path is complete switches to **GET_SHELF**

### <b>4º State: GET_SHELF </b>

In this phase, the shelf is lifted and is deleted from the map. It is important because we get the new state of the warehouse because the shelf is no longer in that position and is going to be in other position.

### <b>5º State: RETURN_SHELF </b>

In this phase, the OMPL algorithm plan a path to place the shelf in a different position. And switches to **GOTO** to reach the new position.

### <b>6º State: LEAVE_SHELF </b>

In this phase, the shelf is put down and the new position is added to the map. And a new shelf is selected to place to a diferrent position.


### <b>OMPL Implementation</b>

The space where the algorithm can plan is define, the start position and the goal position. The state validate function is also define in where we validate if a state is correct or not. In order to check the state, this function checks whether the robot can safely occupy a specific position on the map without colliding with anything. It first transforms the robot’s position and orientation into the map’s coordinate system. Then it estimates the **geometry** the robot (or the robot carrying a shelf) would occupy and verifies whether that space lies within a free area. If any part of that geometry falls outside the map or overlaps with an obstacle, the position is considered invalid. In essence, it is a collision-checking step that ensures the robot only evaluates movements that are **physically feasible**. And the planner used is **RRTstar**, which return the shortest and most optimal path.

- <b>HOLONOMIC </b>

In this case, the space is define by **SE2StateSpace**. And the size of the geometry to check in the validator is different because of robot's size.

- <b>ACKERMANN </b>

In this case, the space is define by **ReedsSheppStateSpace** using a **turning radius**. And the size of the geometry to check in the validator is different because of robot's size.


## <b>Demonstration</b>


- <b>HOLONOMIC </b>

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p4_service/holonomic.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

- <b>ACKERMANN </b>

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p4_service/Ackermann1.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p4_service/Ackermann2.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>