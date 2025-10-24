---
layout: post
title:  "Rescue people"
tags: [service-robotics]
---

## <b>Description</b>

This practice consists of the detection of the survivors in an area with a coverage algorithm and a face detection algorithm.

## <b>Strategy</b>

#### <b>1º Transform from GPS to UTM</b> 

The last coordinate of the survivor are 40º30'29''N 40º30'29''W, but the dron doesn't know about this coordinate system, so the coordinate must be transformed to UTM and metros. 


#### <b>2º Coverage Algorithm</b> 

The coverage algorithm consists of going through a defined area creating squares with increasingly smaller sides.

<div style="text-align: center;">
    <img src="/assets/images/p2_servicios/coverage.jpeg" alt="sin erosión" style="width: 300px; display: inline-block; margin: 0 10px;">
</div>


In order to create a robust algorithm, I defined an huge area where the survivor could be. Then, the area is divided in four quadrants and each quadrant is traversed by the coverage algorithm mentioned above. The center of the area is the last known position of the survivor and the size of each quadrant is determinated by the operator depending on the flight time, the weather... If the size of the area is increased, there are more probabilities of rescuing more people.


<div style="text-align: center;">
    <img src="/assets/images/p2_servicios/quadrants.jpeg" alt= "sin erosión" style="width: 300px; display: inline-block; margin: 0 10px;">
</div>

#### <b>3º Face Detection </b> 

Face detection is made by a module from Open CV called Haar Cascade [1]

[1]: https://docs.opencv.org/4.5.0/db/d28/tutorial_cascade_classifier.html "Haar Cascade: Face Detection"

Some good practices include rotate each frame before trying to detect a face in it. There was also a issue where the algorithm sometimes detected a face in the water, so a color filtered was applied to remove blue tones, making the people detection more accurate.

<p align="center">
  <img src="/assets/images/p2_servicios/salida.gif" width="500">
</p>

#### <b>4º Ficticial battery</b> 

To make the simulation as realistic as possible, the dron's battery is modeled. The dron's battery lasts 15 minutes, and when it runs out, the dron returns to the base on the boat and waits for 5 seconds, to simulate charging.


## <b>Inconvenient and solutions</b>

Because of the quadrants has a significant size, the distance beetween each position is big as well. So I decided to divide the distance creating 
sub-points improving the position control and decreasing the speed between each point.

In order to improve the coverage algorithm and make sure that the dron visits each position commanded, I added a control time to ensure that the dron stays at a point during 0.5 seconds.


## <b>Demostration</b>

Demostration of people's detection.

In the video, you will watch the dron's detection in each quadrant. In the first quadrant, top left, the dron detects 3 people. In the second quadrant, bottom left, the dron detects 1 person and in the third quadrant, top right, the dron detects 2 people. I didn't record the fourth quadrant because of the duration of the video and I knew that the dron had detected the 6 people.

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p2_service/video_cortado.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

Demostration of charging the battery

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p2_service/video_recarga_cortado.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>
