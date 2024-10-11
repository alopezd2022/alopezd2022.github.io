---
layout: post
title:  "P2 Follow Line"
---

## Description
El objetivo de esta practica es realizar un sigue linea que complete el circuito el menor tiempo posible. En este caso, el coche de formula 1 sigue una linea de color rojo y

## Hardware

En esta practica contamos con un coche de formula 1 con camara para seguir la linea.
Por lo que el sensor es la camara y los actuadores son los motores del coche de formula 1.

<div style="text-align: center;">
    <img src="/assets/images/p2/Captura desde 2024-10-11 10-03-10.png" alt="HSV" style= "width: 300px">
</div>

## Software

### Filtrado de color

<div style="text-align: center;">
    <img src="/assets/images/p2/gyuw4.png" alt="HSV" style= "width: 500px">
</div>

### Estrategia 1
Mi primera estrategia fue coger un bounfing box de la imagen y contar cuántos píxeles rojos hay tanto en la izquierda como en la derecha. La diferencia de ambos sería el error de nuestro sistema.
Sin embargo, este algoritmo no era preciso ni robusto, ya que no tenía en cuenta por ejemplo en las curvas donde tanto a izquierda como derecha tenía el mismo número de píxeles.

<div style="text-align: center;">
    <img src="/assets/images/p2/Captura desde 2024-10-11 09-34-36.png" alt="HSV" style= "width: 300px">
</div>

Como esta solución no era valida, decide incrementar el bounding box, cosa que tampoco resulto. Incluso decidi tomar la imagen entera pero seguia sin funcionar por el mismo problema.

### Estrategia 2

Mi segunda estrategia 




<div style="text-align: center;">
    <img src="/assets/images/p2/Captura desde 2024-10-10 17-11-46.png" alt="dibujo" style= "width: 500px">
</div>
## Conclusion
