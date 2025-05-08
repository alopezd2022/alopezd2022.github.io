---
layout: post
title:  "Albor rover"
tags: [modelado-y-simulacion-de-robots]
---
## <b>ALBOR</b>

<div style="text-align: center;">
  <img src="/assets/images/modelado/modelo_renderizado.png" alt="GIF divertido" width="400" />
</div>

&nbsp;

Albor es un robot rover diseñado con un brazo de tipo scara y un compartimento con una capacidad de 3 cubos de 50 cm de arista. Este robot fue diseñado en blender. Y después fue implementado en ros2 para su uso en gazebo harmonic. Para poder utilizar el robot al final del post hay un enlace al github con los dos paquetes indispensables, descargalos y compilalos en tu workspace de ros jazzy.

Para la realizacion de este robot se ha hecho uso de:
- Blender con phobos.
- [Gazebo harmonic](https://gazebosim.org/docs/latest/getstarted/)
- [Moveit](https://moveit.picknik.ai/main/index.html)
- [Teleoperar el robot en gazebo](https://wiki.ros.org/teleop_twist_keyboard)

&nbsp;

## <b>Arbol de transformada</b>

<div style="text-align: center;">
  <img src="/assets/images/modelado/Captura desde 2025-05-08 10-30-38.png" alt="GIF divertido" width="1000" />
</div>

&nbsp;

<div style="text-align: center;">
  <img src="/assets/images/modelado/Captura_parteA.png" alt="GIF divertido" width="1000" />
</div>

&nbsp;

## <b>Analisis gráfico</b>

### Gráfica Posición de las ruedas vs Tiempo

<div style="text-align: center;">
  <img src="/assets/images/modelado/tiempo_vs_posicionruedas.png" alt="GIF divertido" width="400" />
</div>
&nbsp;

En el eje y se encuentra la posición en metros y en el eje x se encuentra el tiempo en segundos.

En esta grafica podemos observar que la posición de las ruedas no varia hasta que iniciamos el movimiento del robot para aproximarnos al cubo. Una vez que nos aproximamos al cubo, a la posición para realizar el pick and place, se mantiene en esa posición.

&nbsp;

### Gráfica Aceleración vs Tiempo

<div style="text-align: center;">
  <img src="/assets/images/modelado/tiempo_vs_aceleracion.png" alt="GIF divertido" width="400" />
</div>

&nbsp;

En el eje y se encuantra la aceleración lineal en m/s² y en el eje x se encuentra el tiempo en segundos.

En esta grafica podemos ver dos diferencias principales, por un lado las aceleraciones en el eje 'x' y en el eje 'y' y por otro lado las aceleraciones en el eje z. 

- La aceleración en el eje x observamos que es cero al principio de la ejecución, entre los segundos 30 y 60 aproximadamente se realiza un movimiento en este eje, esto se debe a las oscilaciones que podemos observar.Y Una vez acabado el moviento vuelve a cero.

- La aceleración en el eje y se comporta de la misma manera que la aceleración en el eje x.

- La aceleración en el eje z se encuentra constante en 10 m/s² esto se corresponde a la gravedad ya que el eje z apunta hacia arriba.

&nbsp;

### Gráfica Gasto vs Tiempo

<div style="text-align: center;">
  <img src="/assets/images/modelado/gasto_vs_tiempo.png" alt="GIF divertido" width="600" />
</div>

&nbsp;

En esta grafica observamos primeramente, desde casi 400 hasta 415, se realiza un primer moviento correspondiente a estirar el brazo scara para posicionar el brazo para coger el cubo. La siguiente oscilacion corresponde a otro moviento del bazo para posicionarlo en posicion para depositar el cubo en el compartimento del rover y volver a la posicion anterior. La siguiente oscilación es negativa y corresponde a la fuerxa negativa para bajar el brazo y agarrar el cubo y la oscilación posistiva corresponde a volver a subir el brazo con el cubo agarrado.

&nbsp;

## <b>Video de ejecución</b>

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/modelado/video.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

&nbsp;
&nbsp;

#### Rosbag
[enlace de descarga del rosbag](https://urjc-my.sharepoint.com/:f:/g/personal/a_lopezd_2022_alumnos_urjc_es/EsfUU-JLf9REudeLFno0jcUBakbcHoI9GJ1tpp3oV6GYgg?e=yLijZk)
#### Github
[enlace del github de la practica](https://github.com/alopezd2022/albor_repo.git)

&nbsp;

