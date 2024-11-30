---
layout: post
title:  "P3 Vending machine"
tags: [embedded-and-real-time-systems]
---

## Description
La practica consiste en diseñar un controlador para una maquina expendedora que este basado en Arduino Uno. Para diseñar este controlador haremos uso de Arduino Threads, interrupciones, watchdog... 

## Hardware


<p align="center">
  <img src="/assets/images/p3-empotrados/arduino.jpeg" alt="Arduino UNO" width="200">
  <img src="/assets/images/p3-empotrados/lcd.webp" alt="LCD" width="200">
  <img src="/assets/images/p3-empotrados/joystick.jpg" alt="Joystick" width="200">
</p>
<p align="center">
  <b>Arduino UNO</b> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <b>LCD</b> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <b>Joystick</b>
</p>

<p align="center">
  <img src="/assets/images/p3-empotrados/dht11.jpeg" alt="Sensor DHT-11" width="200">
  <img src="/assets/images/p3-empotrados/Hc-sr04.jpg" alt="Sensor HC-SR04" width="200">
  <img src="/assets/images/p3-empotrados/boton.jpeg" alt="Botón" width="200">
</p>
<p align="center">
  <b>Sensor DHT-11</b> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <b>HC-SR04</b> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <b>Botón</b>
</p>

<p align="center">
  <img src="/assets/images/p3-empotrados/leds.jpg" alt="LED Rojo" width="200">
</p>
<p align="center">
  <b>LEDs</b> 
</p>

&nbsp;

A partir de todos estos componentes obtenemos el siguiente esquema electrónico.

<p align="center">
  <img src="/assets/images/p3-empotrados/Maquina_expendedora_bb.jpg" width="500">
</p>

## Software

### Clases
#### Boot

La clase boot es la clase encargada del arranque de la maquina expendedora. La clase Boot es una clase derivada de Thread que controla un pin led y una pantalla LCD, por lo tanto se ejecuta como "hilo independiente" que, una vez que acaba la secuencia de arranque, no se vuelve a ejecutar.

Al ejecutar se llama al metodo run la cual va alternando el estado de un pin y además muestra por la pantalla del lcd la cadena "CARGANDO...".

Cuando el contador ha llegado a 3, se dejada de ejecutar el hilo y se remueve del controlador, ya que no volveremos a ejecutar dicha clase.

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3-empotrados/boot.mp4' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

&nbsp;

#### Sensors

La clase sensor es la clase encargada de la lectura del sensor de humedad y temperatura y humedad. La clase es una clase derivada de Thread, por lo que se ejecuta como "hilo" cada 5 segundos y nunca es removido debido a que siempre queremos tener monitorizado la temperatura y humedad de nuestra maquina para prevenir de posibles desastres.

En ella encontramos la inicialización del sensor, dos métodos que obtienen los valores de temperatura y humedad respectivamente y dos métodos que muestran los valores por el lcd.

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3-empotrados/sensors.mp4' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

&nbsp;

#### Menu

La clase Menu es una clase que hereda de Thread y administra un menú de bebidas para una pantalla LCD controlada por un objeto LiquidCrystal. Contiene un arreglo de objetos Item (con nombre y precio), índices para la navegación del menú, y métodos para mostrar el menú en pantalla, cambiar precios y apagar la pantalla. El constructor inicializa el menú con 5 elementos predeterminados y configura el índice inicial en 0. 

Los métodos permiten visualizar el elemento actual del menú, actualizar su precio y limpiar la pantalla LCD.

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3-empotrados/menu.mp4' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

&nbsp;

#### Admin

La clase Admin hereda de Thread y gestiona un menú administrativo para un dispositivo con pantalla LCD controlada por un objeto LiquidCrystal. Contiene un arreglo de cadenas con 4 opciones de menú y un índice para navegar entre ellas.

Incluye un método para mostrar la opción actual del menú en la pantalla y otro para apagar (limpiar) la pantalla. Proporciona una interfaz simple para administrar funcionalidades como sensores y configuración.

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3-empotrados/admin.mp4' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

&nbsp;

### Main

El main esta compuesto principalmente de las funciones :

- **setup** en donde se inicializa el lcd, los pines para los leds y para las interrupciones, los intervalos de los threads en el controlador y el watchdog.

- **loop** en donde se encuentra una maquina de estados finita.

&nbsp;

<p align="center">
  <img src="/assets/images/p3-empotrados/state-machine.jpg" width="500">
</p>

&nbsp;

Como principales estados tenemos:

- **BOOT**, estado que obtiene el contador de la clase BOOT y si ha llegado a 3 termina y transita al estado WAITING_CLIENT

- **WAITING_CLIENT**, estado en el que el sensor de ultrasonidos recoge los valores de la distancia, en cuando hay un cliente a menos de un metro transita al estado IS_CLIENT

- **IS_CLIENT**, estado que muestra durante 5 segundos la temperatura y humedad y transita al estado SHOW_MENU.

- **SHOW_MENU**, estado que muestra el menú usando el método "showMenu" de la clase Menu, solo transita unicamente con la interrupción del botón del joystick.

- **PREPARE_PRODUCT**, estado que simula la preparación del producto, en el se enciende progresivamente un led y muestra un mensaje por el lcd durnate un tiempo ramdon. Una vez finalizado muestra por el lcd que el producto esta listo y transita al estado WAITING_CLIENT.

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3-empotrados/prepare-product.mp4' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>

&nbsp;

En cualquier momento si queremos reiniciar y volver al estado WAITING_CLIENT se debe pulsar el botón en un rango de entre 2 y 3 segundos.

Con estos 5 estados tenemos el comportamiento a nivel de usuario de la maquina expendedora. Sin embargo, hay más comportamientos a nivel de administrados. Para transitar al estado de administrador se debe pulsar el botón durante 5 segundos.

- **ADMIN**, estado que muestra el menú del administrados con las diferentes funcionalidades. Cada una se muestra como un sub-estado:

  - **SEE_TEMPERATURE**, estado que muestra la temperatura y la humedad.

  - **SEE_DISTANCE**, estado que muestra los valores del sensor de ultrasonidos

  - **COUNTER**, estado que muestra el tiempo desde que se inicio.

  - **CHANGE_PRICE**, estado que cambia el precio de un producto, el cual tiene el sub-estado **ASIGN_PRICE** al cual solo transita unicamente con la interrupción del boton del joystick.

Para transitar por los diferentes sub-estados al estado ADMIN unicamente se puede hacer mediante un movimiento del joystick en el eje X a la izquierda, una acción para "volver al menú"

&nbsp;

#### Watchdog 

Implementamos el watchdog como mecamismo de seguridad por si el sistema se queda bloqueado y no tiene el comportamiento deseado con un timer de 8 segundos.

Podemos observar su correcto funcionamiento ya que en el estado PREPARE_PRODUCT simulamos la preparación del producto con un delay random entre 4 y 8 segundos. Si no se resetea el watchdog, este va a reiniciar el sistema cuando el random de valor 8. En el siguiente video se puede observar lo comentado anteriormente.

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3-empotrados/watchdog.mp4' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>
&nbsp;

### Final video

Video final con el comportamiento completo de la Maquina expendedora.

<div style="text-align: center;">
    <video width="500" controls>
      <source src="{{ '/assets/videos/p3-empotrados/complete-video.mp4' | relative_url }}" type="video/webm">
      Tu navegador no soporta la reproducción de videos.
    </video>
</div>


