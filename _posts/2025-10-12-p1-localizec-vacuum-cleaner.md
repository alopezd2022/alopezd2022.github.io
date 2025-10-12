---
layout: post
title:  "Localized Vacuum Cleaner"
tags: [service-robotics]
---
## <b>Description</b>
In this practice, a **BSA (Backtracking Spiral Algorithm)** coverage algorithm is implemented and applied to the autonomous navigation of a vacuum robot. The goal is to achieve complete coverage of the free area while avoiding obstacles and optimizing the robot’s movements.

## <b>Strategy</b>

#### <b>1º Map record</b> 

This first step consists of obtaining the relation between the pose 3d of the vacuum in the world and the pose 2D of the vacuum in the map.

This relation lets us convert from pose 3D to pose 2D and vice versa. In order to get this relation we need to calculate the transformation matrix.
Since the vacuum robot isn't at the center of the map we can not create a simple relation and obtain a matrix from the rotation of the axes from world to map.

Therefore, we need to get some points (many as we can) in 3D and the corresponding points in 2D so we can calculate least square an get the matrix which relates both sets of points. The more points we get, the less error is generated. 


#### <b>2º Creating a navigation grid map</b> 

The second step consists of creating a grid map. Each grid cell has a slightly smaller than the robot's size in pixels. Having the size of each grid cell we can determinate the size of the navigation grid map. Each grid have a value depending on the states of the grid, if the grid cell corresponds to a obstacle the value is 0, if the grid cell is free, the value is 1 and if the robot has been in that grid cell, the value is 2. 


#### <b>3º Implementation of a bsa coverage algorithm</b> 

*The coverage algorthim uses a grid-based model, and assures the complete coverage of non-occupied cells; partially occupied cells are not considere, is supported by two main concepts: covering of simple regions using a spiral-like path and linking of simple regions based on a backtracking mechanism* [1]

As the article said, this algorthim uses a coverage based on spiral that goes around all the free cell. But what happens when the robot ends a spiral and all the surrounding cells are occupied?. This algorithm also implements critical points and return points. A critical point is a point without an exit, and a return point is the closest free point to the critical point. So when the robot is at a critical point, it must move to a return point and continue the algorithm from there.

In order to get the closest return point and the path to this point, we need to implement a backtracking mechanism such as BFS Algorithm, Breadth-First Search.
Breadth-First Search consists of expanding an unvisited node and its successors until the goal is reached. In this case the goal is all the return points, and the node start in the critical point and go around each cell and its successors until a cell which is a return point is found. This allows obtaining the closest return point and the full path from the critical point to the return point.

[1]: https://www.academia.edu/103916763/BSA_A_Complete_Coverage_Algorithm "BSA: A Complete Coverage Algorithm"

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p1_service/bsa.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>

</div>


#### <b>4º Reactive piloting according to the planned route</b> 

The piloting of the robot involves simple movements in which it is controlled by its position, in each iteration there is a new goal, a new grid in the map to move to. The robot is controlled by a simple P control with proportional gains applied to the error both in the angle and in the distance to the goal.

## <b>Inconvenient and solutions</b>

To obtain the points to create the transformation matrix, I initially obtain ten points, which were sufficient to create the navegation grid and to run the BSA algorithm. However, when the robot had to move, the transformation matrix wasn't accurate enough and the robot collided at certain points. The solution was to obtain more points to create an accurate matrix. The points were the robot collided were the one that I used to improve the matrix.

Also regarding to obstacle avoidance, a good approach is to dilate the obstacles in the map so the robot won't collide with them in the real world. Therefore, I apply an erosion operation to the map, resulting in less white area and more black areas.

<div style="text-align: center;">
    <img src="/assets/images/p1_servicios/mapa_sinerosion.png" alt="sin erosión" style="width: 300px; display: inline-block; margin: 0 10px;">
    <img src="/assets/images/p1_servicios/mapa_erosionado.png" alt="erosionado" style="width: 300px; display: inline-block; margin: 0 10px;">
</div>



## <b>Demostration</b>

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p1_service/output_4x_largo.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>