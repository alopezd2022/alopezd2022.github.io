---
layout: post
title:  "P2 Follow Line"
tags: [robotica-movil]
---

## Description
The objective of this practice is to create a line follower that completes the circuit in the shortest possible time. In this case, the Formula 1 car follows a red line, and a PD controller will be used to control the angular and linear speed of the system.

## Hardware

In this practice, we have a Formula 1 car equipped with a camera to follow the line. Therefore, the sensor is the camera, and the actuators are the motors of the Formula 1 car.

<div style="text-align: center;">
    <img src="/assets/images/p2/Captura desde 2024-10-11 10-03-10.png" alt="HSV" style= "width: 300px">
</div>

## Software

### The color filtering

The color filtering is done using HSV, a system in which I can adjust the color range (in this case, red), brightness, and saturation of the color. With these three parameters, I can accurately control all shades of red, avoiding any external elements such as the incidence of unexpected light.

<div style="text-align: center;">
    <img src="/assets/images/p2/gyuw4.png" alt="HSV" style= "width: 500px">
</div>
&nbsp;
### Strategy 1
My first strategy was to take a bounding box from the image and count how many red pixels there were on both the left and right sides. The difference between them would represent the error of our system. However, this algorithm was neither precise nor robust, as it did not account for situations like curves, where both the left and right sides could have the same number of pixels.

<div style="text-align: center;">
    <img src="/assets/images/p2/Captura desde 2024-10-11 09-34-36.png" alt="HSV" style= "width: 350px">
</div>

Since this solution was not valid, I decided to increase the size of the bounding box, but that didn’t work either. I even tried using the entire image, but it still didn’t work due to the same problem.

&nbsp;
### Strategy 2

The second strategy consists of taking two lines, one horizontal and one vertical. For each line, the average of the red pixels at that moment is calculated. The point obtained from these two averages is the point the car should always follow, meaning it represents where the car should always be.

Therefore, the error in our system is the difference between the fixed center point of the image (the intersection of the horizontal and vertical lines) and the point obtained from the average of the red pixels, which indicates where the car should be. However, I chose to divide the error into two distinct errors, as the difference along the horizontal axis indicates how much the car needs to turn, while the difference along the vertical axis indicates how much the car needs to move forward.

With two errors—one for angular velocity and one for linear velocity—we obtain two different controllers, making our strategy more precise and robust.

The blue point seen in the image represents the average position of the red pixels in both the horizontal and vertical directions, while the drawn cross is the point where the car should be.

Additionally, when the car loses the line, it turns to the last saved angle.

<div style="text-align: center;">
    <img src="/assets/images/p2/Captura desde 2024-10-10 17-11-46.png" alt="HSV" style= "width: 350px">
</div>

&nbsp;
### Control

We are going to implement a PD controller, which is a feedback control mechanism that adjusts the output based on the error obtained from the difference between the desired value and the current value. This controller uses two components to reduce the error and achieve a better system response: the proportional part and the derivative part.

Proportional

&nbsp;
The proportional part adjusts the output in proportion to the error; that is, if the error is large, the controller's output will also be large. 

It is calculated as the error multiplied by the proportional constant Kp. The constant determines the intensity of the system's output relative to the error.

Derivative
&nbsp;

The derivative part measures the change in the error over time. It assesses how quickly the error is changing and adjusts the output based on that change.

It is calculated as the derivative of the error multiplied by a derivative constant.

### Result

After explaining the filtering, tracking algorithm, and control, the result is a Formula 1 car that completes the circuit in approximately 100 seconds.

<div style="text-align: center;">
    <video width="400" controls>
      <source src="{{ '/assets/videos/p2/cochesimple.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

## Conclusion

Clearly, this algorithm is not the fastest, but in my opinion, it is quite robust and effective. I wasn't looking for a fast car; I aimed for a car with control that has "smooth" movements, meaning it experiences minimal oscillation. As a final result, the car shows little oscillation and maintains control over both linear and angular velocity, which seems like a good outcome.
