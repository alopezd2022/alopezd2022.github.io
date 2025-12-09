---
layout: post
title:  "Autonomous Frontier Exploration"
tags: [service-robotics]
---

## <b>Description</b>

This practice focuses on programming a **robot capable of autonomously exploring an environment** using **occupancy grid mapping** and **frontier-based exploration**.

The robot uses its **laser sensor** to detect obstacles and free space, updating a **probabilistic map** in real-time using **log-odds representation**. The exploration strategy relies on detecting **frontiers**, which are the boundaries between known free space and unknown regions, to dynamically generate exploration targets.

## <b>Strategy</b>

The overall operation of the system is implemented as a **state machine** with two main states:

### <b>1º State: UPDATE </b>

In this phase, the robot:

- Retrieves the **current odometry** and **laser scan data**.
- Updates the **occupancy grid** using a log-odds approach.
- Detects **frontiers** in the map by searching for cells that border unknown areas.
- If a valid frontier is found, the system switches to the **MOVE** state to navigate towards it. Otherwise, the robot moves forward safely or rotates if an obstacle is detected.

### <b>2º State: MOVE </b>

In this phase, the robot:

- Follows a path generated from the current position to the selected frontier using a simple **proportional controller**.
- Checks continuously whether the **path and surrounding cells are safe** before moving.
- Stops when the target is reached and returns to the **UPDATE** state to detect new frontiers.


## <b>Mapping Implementation</b>

The map is represented as a **2D grid of log-odds values**, which are updated as follows:

- **Occupied cells** (obstacles detected by the laser) increase the log-odds by log_odds_hit.
- **Free cells** (laser passes through them) decrease the log-odds by log_odds_free.
- The probability of occupancy is obtained via the **sigmoid function**:
<div style="text-align: center;">
p = 1 - 1 / (1 + e^l)
</div>
where \(l\) is the log-odds value.

The **Bresenham algorithm** is used to efficiently trace the line between the robot and the detected obstacle, marking free cells along the path.



## <b>Frontier Detection</b>

Frontiers are detected by exploring the free space and identifying cells that are **adjacent to unknown areas**. The algorithm:

1. Starts from the robot’s current position.
2. Uses a **breadth-first search** to explore free cells.
3. Detects cells at the boundary of unknown space as frontiers.
4. Validates safety by checking a **local square around the frontier** for obstacles.

Once a valid frontier is found, the robot generates a **path** towards it and switches to the **MOVE** state.



## <b>Control and Navigation</b>

The robot uses a **reactive proportional controller** to navigate:

- If the **angle error** to the target is large, the robot rotates in place.
- If the path is clear, the robot moves forward.
- The controller ensures that the robot **avoids collisions** and reaches the target safely.

Additionally, the robot checks if the **forward path is clear** using the laser scanner, adjusting its movement dynamically.



## <b>Demonstration</b>

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p5_service/output_fast.mp4' | relative_url }}" type="video/mp4">
      Your browser does not support the video tag.
    </video>
</div>

This demonstration shows the robot autonomously exploring an unknown environment, updating its occupancy map, and navigating safely to discovered frontiers.
