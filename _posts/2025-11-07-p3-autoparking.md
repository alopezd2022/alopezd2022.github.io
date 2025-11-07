---
layout: post
title:  "Autonomous Parking"
tags: [service-robotics]
---

## <b>Description</b>

This practice consists of developing an **autonomous system** capable of **detecting a parking space** and **performing the parking maneuver** without human intervention.
For this purpose, data from **laser sensors** located at the **front**, **rear**, and **sides** of the vehicle are used, along with **speed (V)** and **steering (W)** control.

The algorithm implements different **behavioral states**, including: **space search**, **alignment**, **parking space type identification**, and **parking maneuvers** depending on the case.


## <b>Strategy</b>

The overall operation of the system is based on a **state machine** that changes dynamically according to the **sensor readings**.
Below are the main **modules** and **phases** of the practice.


### <b>1º State: SEARCHING </b>

In this phase, the robot moves forward in a straight line while keeping itself aligned with the parked cars on its right.
To achieve this, the `get_align()` function compares the **symmetric values** from the **right-side sensor** to correct small deviations through the steering control `HAL.setW(w)`.

At the same time, the `search_parking()` function evaluates whether there is a sufficiently large space to park between two vehicles, using a geometry based on the **Pythagorean theorem**. Through this method, the **angles** and the **longest side** of the detected rectangle are calculated and compared with the **laser readings** to determine whether the space is free or not.
When a valid space is detected, the robot stops its forward motion and switches to the **IDENTIFY** state.



### <b>2º State: IDENTIFY </b>

Once a potential parking space is detected, the system analyzes the distances at the **extreme angles** of the space (`min_angle` and `max_angle`) to determine the **type of space**:

* **Type 0:** there is a car in front and behind (intermediate space).
* **Type 1:** no car in front (open space at the front).
* **Type 2:** no car behind (open space at the rear).

This identification is crucial to decide **the direction of the maneuver** and the **movement mode** (forward or reverse).


### <b>3º State: ALIGN </b>

In this state, the vehicle adjusts its position to become **parallel to the parked cars** before starting the maneuver.
The `align_with_car()` function once again uses data from the **right-side sensor** to maintain alignment and stops the movement once the **geometry of the space** matches the expected configuration.


### <b>4º State: MANOEUVRE </b>

This state covers both **parking with a car in front and another behind** (type 0), and **parking with only a car in front** (type 2), since the movements are essentially the same.
The only difference lies in the **reference system** used when reversing:
when there are cars both in front and behind, the **car behind** is used as a reference while backing up; however, when there is only a car in front, that **front car** becomes the reference point.

The parking process is divided into **substates** that simulate a real parking maneuver:

1. **Reverse:** the vehicle steers backward at a certain angle until it detects an increase in the lateral distance.
2. **Forward:** it continues reversing while correcting the steering angle until it reaches a minimum safe distance from the car behind or the car in front.
3. **Straighten:** it moves slightly forward while straightening the wheels to end up fully aligned within the parking space.

At the end of this sequence, the vehicle is properly parked and stops.



### <b>5º State: MANOUVRE_1 </b>

If the detected space is **open at the front** (type 1), the maneuver differs slightly:

* The vehicle moves forward while turning to the right.
* When the lateral space increases enough, it straightens the wheels.
* Then, it reverses to center its position within the parking space.

This procedure ensures **efficient and safe parking**, adapting to the different types of spaces encountered.

## <b>Inconvenients and Solutions</b>


* **Infinite sensor readings:** when the lasers did not detect any obstacles, they returned `inf`. This was solved using the `clean()` function, which replaces these values with the maximum measurable range.
* **Unstable alignment:** small oscillations were observed when trying to maintain parallelism with the parked cars. This was stabilized by adjusting the proportional factor `k` of the error control.
* **Sudden substate changes:** distance thresholds (`threshold_distance`) and short time delays were added to prevent incorrect transitions between phases.


## <b>Demonstration</b>

In the following videos, the complete sequence can be observed: **space search**, **identification**, and **parking maneuver**.

**Parking with a car in front and another behind**

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3_service/aparcar_delante_atras_recortado.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

**Parking with only a car behind**

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3_service/aparcar_para_atras_recortado.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

**Parking with only a car in front**

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3_service/aparcar_delante_recortado.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>
