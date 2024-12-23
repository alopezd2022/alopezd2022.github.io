---
layout: post
title:  "P4 Follow Line"
tags: [embedded-and-real-time-systems]
---
# <b>RUST-EZE</b>
## <b>Descripción</b>

El objetivo de esta practica es diseñar un sigue lineas incorporando comunación IoT, controladores, Arduino Threads...

## <b>Hardware</b>

Para este practica se ha usado un Arduino UNO, una ESP32, un sensor de ultrasonidos HC-SR04, un sensor de infrarrojos y los actuadores en cuestión, los motores para las ruedas del coche.

## <b>Software</b>

Para esta practica hemos obstado por continuar con el sistema operativo de Arduino y no implementar FREE-RTOS. Esto se debe a que esta practica no precisa de un manejo estricto de tareas que deban cumplir deadlines. Con el único hilo y flujo de ejecución de Arduino se pueden conseguir los objetivos principales, seguir la linea leyendo en cada iteración el sensor de infrarrojos, enviar a la ESP la información necesaria mediante el puerto serie y verificar el sensor de ultrasonidos en cada iteración para la detección de obstáculos.

### <b> Comunicación </b>
#### <b>Serie entre Arduino y ESP</b>
La comunicación por los puertos serie entre el Arduino y el ESP es una comunicación bidireccional, en la que la ESP inicia la comunicación indicando que se puede iniciar la carrera una vez que se ha conectado al WIFI y al broker MQTT. Cuando la ESP envía el mensaje de inicio, el Arduino inicia un paso de mensajes dependiendo de la acción que realiza el coche. Las acciones pueden ser :
 - START_LAP, inicio de la vuelta.
 - END_LAP, fin de la vuelta, además envía el tiempo que ha tardado en dar la vuelta.
 - OBSTACLE_DETECTED, detección de obstáculo, envía a que distancia ha detectado el obstáculo.
 - LINE_LOST, perdida de linea.
 - PING, cada 4 segundos se envía este mensaje junto con el tiempo transcurrido.
 - LINE_FOUND, recuperación de linea.
 - VISIBLE_LINE, se envía el porcentaje de linea que se ha visto durante la vuelta.
 - INIT_LINE_SEARCH, inicia el algoritmo de búsqueda de linea.
 - STOP_LINE_SEARCH, finaliza el algoritmo de búsqueda de linea.
 
Para enviar todos estos mensajes, el arduino sigue el siguiente protocolo diseñado específicamente para la comunicación Arduino-ESP: <i>{/action:=$ACCION\}</i>. Si ademas se quiere enviar algún dato más se añade tras la acción : <i>{/action:=$ACCION\/time:=$TIME\}</i>

#### <b>Comunicación IoT a través de MQTT</b>

Cuando la ESP32 recibe los distintos mensajes por el puerto serie esta recoge los mensajes, obtiene los datos necesarios y los envía en formato JSON al servidor MQTT en donde se procesan los datos y se lleva un control remoto de la vuelta.

### <b>PID</b>

El control PID (Proporcional-Integral-Derivativo) es un algoritmo ampliamente utilizado en sistemas de control para ajustar una variable de salida en función de un valor deseado. En esta práctica, se utiliza un control proporcional-derivativo (PD), para guiar el movimiento del coche mientras sigue una línea negra detectada por sensores de infrarrojos.

El sistema de control PD se basa en los siguientes principios:

#### <b>Componente Proporcional (P):<b> 

Calcula una corrección basada en el error actual. En este caso, el error se define como la diferencia en intensidad de los valores de los sensores de infrarrojos a ambos lados de la línea. Este componente ajusta directamente las velocidades de los motores en función de la desviación de la línea, proporcionando una corrección inmediata.

#### <b>Componente Derivativo (D):<b> 

Ajusta la corrección en función del cambio del error en el tiempo. Este componente ayuda a suavizar los movimientos, evitando oscilaciones bruscas al anticipar el comportamiento del coche.

En el contexto de esta practica, el algoritmo realiza los siguientes pasos:

#### <b>1.Lectura de Sensores:<b> 

Los sensores de infrarrojos detectan la intensidad de la línea en tres puntos: izquierda, centro y derecha. Según los valores recibidos, el sistema determina si el coche está alineado con la línea, desviado a la izquierda, o desviado a la derecha.

#### <b>2.Cálculo del Error:<b> 

Se calcula el error como la diferencia en las lecturas de los sensores izquierdo y derecho. Este error indica qué tan lejos está el coche de estar perfectamente alineado con la línea.
#### <b>3.Aplicación del Control PD:<b>

 - El error proporcional se multiplica por una constante de ganancia proporcional (Kp), lo que genera una corrección proporcional a la magnitud del error.
 - El cambio del error (derivada del error) se multiplica por una constante de ganancia derivativa (Kd​), que suaviza las correcciones para evitar oscilaciones.

#### <b>4.Ajuste de Velocidades de los Motores:<b> 

La corrección calculada se aplica para modificar las velocidades de los motores izquierdo y derecho, de manera que el coche pueda ajustar su trayectoria y mantenerse sobre la línea.

#### <b>5.Limitación de Velocidades:<b>

Las velocidades calculadas se limitan para evitar valores fuera del rango permitido, asegurando un movimiento estable y seguro.

## Video

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/follow_line/coche-clase.mp4' | relative_url }}" type="video/mp4">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

&nbsp;

Colaborador Jorge Barroso Saugar
